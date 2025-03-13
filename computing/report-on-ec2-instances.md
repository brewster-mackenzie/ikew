# Report on EC2 instances

#aws #ec2 #powershell

-----

Use PowerShell to generate a CSV report of all EC2 instances in the given region, with the instance ARN, account ID and other properties.

Replace `$region` in the script to select the region for which the report should be generated.


```powershell
# Region
$region = 'eu-west-1'

# Initialize variables for pagination
$nextToken = $null
$allResources = @()

do {
    # Build the command with pagination support
    $command = "aws resource-explorer-2 search --region $region --query-string 'ResourceType:ec2:instance' --output json --no-paginate"
    if ($nextToken) {
        $command += " --next-token $nextToken"
    }

    # Execute the command and parse the output
    $response = Invoke-Expression $command | ConvertFrom-Json

    # Append the resources to the list
    $allResources += $response.Resources

    # Get the next token for pagination
    $nextToken = $response.NextToken
} while ($nextToken)

# Check if resources were found
if ($allResources.Count -eq 0) {
    Write-Error "No resources found."
    exit
}

# Log the number of resources found
Write-Output "Number of resources found: $($allResources.Count)"

# Prepare an array to hold the combined data
$csvOutput = @()

# Loop through each resource and extract necessary details
foreach ($resource in $allResources) {
    try {
        # Extract basic details
        $basicDetails = $resource | Select-Object ARN, OwningAccountId, Region

        # Log basic details
        Write-Output "Processing resource with ARN: $($basicDetails.ARN)"

        # Extract properties
        $properties = $resource.Properties | Select-Object Data

        # Extract instance ID from ARN and log it
        Write-Output "Extracting instance ID from ARN: $($basicDetails.ARN)"
        try {
            $instanceId = ($basicDetails.ARN -split "/")[1]
            Write-Output "Extracted instance ID: $instanceId"
        } catch {
            Write-Error "Failed to extract instance ID from ARN: $($basicDetails.ARN)"
            continue
        }

        # Fetch instance tags using the correct region
        try {
            $region = $basicDetails.Region
            Write-Output "Fetching tags for instance ID: $instanceId in region: $region"
            $instanceTagsJson = aws ec2 describe-instances --instance-ids $instanceId --region $region --query "Reservations[*].Instances[*].Tags" --output json
            $instanceTags = $instanceTagsJson | ConvertFrom-Json

            # Error handling for instance tags
            if ($instanceTags -eq $null -or $instanceTags.Count -eq 0) {
                $instanceTags = @()
                Write-Output "No tags found for instance ID: $instanceId in region: $region"
            } else {
                $instanceTags = $instanceTags[0][0]
                Write-Output "Tags found for instance ID: $instanceId in region: $region"
            }

            # Convert tags to a hash table for easier CSV handling
            $tagsHashTable = @{}
            foreach ($tag in $instanceTags) {
                $tagsHashTable[$tag.Key] = $tag.Value
            }

            # Combine basic details, properties, and tags
            $combinedData = [PSCustomObject]@{
                ARN             = $basicDetails.ARN
                OwningAccountId = $basicDetails.OwningAccountId
                Region          = $basicDetails.Region
                Properties      = $properties.Data
            }

            # Add tags to the combined data
            foreach ($key in $tagsHashTable.Keys) {
                Add-Member -InputObject $combinedData -MemberType NoteProperty -Name $key -Value $tagsHashTable[$key]
            }

            # Add the combined data to the output array
            $csvOutput += $combinedData
        } catch {
            Write-Error "Failed to fetch instance tags for instance ID: $instanceId in region: $region. Error: $_"
            continue
        }
    } catch {
        Write-Error "An error occurred while processing resource with ARN: $($basicDetails.ARN). Error: $_"
        continue
    }
}

# Export the combined data to a CSV file
$csvOutput | Export-CSV -Path .\ResourceList.csv -NoTypeInformation

# Log completion
Write-Output "Script execution completed. CSV file created at .\ResourceList.csv"
```
