# Report on AWS Inspector vulnerabilities

Use PowerShell to generate a CSV report of all active AWS Inspector vulnerabilities.  

Modify the `$reports` variable to include one or more report to generate, replacing the `vpcId`, `bucketName` and `kmsKeyArn` as required.

The reports will be stored in S3 and then downloaded to the local machine for some final processing.  An additional Summary report will also be generated, containing the total count and maximum age of all Critical and High vulnerabilities.


```powershell
$reports = @(
    @{ 
        region='eu-west-1'; 
        vpcId='<insert-vpc-id>'
        bucketName='<insert-bucket-name>';
        kmsKeyArn='<insert-key-arn>';
    } # , @{ ... }, @ { ... }, etc.
) 

$summaryData = @()
$summaryFileName = Join-Path $pwd 'InspectorReportSummary.csv'

foreach($report in $reports) {
    Write-Host "generating report for $($report.vpcId) in $($report.region)"
    $report.name = $vpcId
    $json = aws ec2 describe-vpcs --region $report.region --vpc-ids $report.vpcId
    if ($json) {
        $tag = ($json | ConvertFrom-Json).Vpcs[0].Tags | Where-Object Key -EQ Name
        if ($tag) {
            $report.name = $tag.Value
        }
    }

    $report.name = "InspectorReport_$($report.region)_$($report.name)"
    $report.filename = Join-Path $pwd "$($report.name).csv"

    Write-Host "create findings report $($report.name) in bucket $($report.bucketName)"
    $json = aws inspector2 create-findings-report --region $report.region --report-format CSV --s3-destination "bucketName=$($report.bucketName),kmsKeyArn=$($report.kmsKeyArn)" --filter-criteria ec2InstanceVpcId="{comparison='EQUALS',value=$($report.vpcId)}"
    if (!$json) {
        Write-Error "failed to create report $($report.name) - response was empty"
        continue
    }

    $report.reportId = $json | ConvertFrom-Json | Select-Object -ExpandProperty reportId
    
    Write-Host "waiting for findings report $($report.name) to be ready..."
    $report.ready = $false
    $retry=5
    while($retry--) {
        $json = aws inspector2 get-findings-report-status --region $report.region --report-id $report.reportId
        if ($json) {            
            $status = $json | ConvertFrom-Json | Select-Object -ExpandProperty Status
            if ($status -eq 'SUCCEEDED') {
                $report.ready = $true
                break
            }
        }

        Start-Sleep -Seconds 10
    }        

    if (!$report.ready) {
        Write-Error "failed to create report $($report.name) - report was not ready within the time limit"
        continue
    }

    Write-Host "downloading report $($report.name)"
    $s3uri = "s3://$($report.bucketName)/$($report.reportId).csv" 
    aws s3 cp $s3uri $report.filename | Out-Null
    $report.downloaded = $LASTEXITCODE -eq 0

    if (!$report.downloaded) {
        Write-Error "failed to download report $($report.name) with id $($report.reportId)"
        continue
    }
    
    Write-Host "gathering data from report $($report.name)"    
    $sevs = Get-Content $report.filename | ConvertFrom-Csv | Group-Object Severity
    $sevC = $sevs | Where-Object Name -EQ 'CRITICAL'
    $sevH = $sevs | Where-Object Name -EQ 'HIGH'
    $sevM = $sevs | Where-Object Name -EQ 'MEDIUM'
    $ageField = 'Age (Days)'

    $summaryData += @([PSCustomObject][ordered]@{
        Region=$report.region;
        VpcId=$report.vpcId
        Name=$report.name;
        CriticalCount=$sevC.Count;
        CriticalMaxAge=($sevC.Group | Measure-Object $ageField -Maximum).Maximum
        HighCount=$sevH.Count;
        HighMaxAge=($sevH.Group | Measure-Object $ageField -Maximum).Maximum;
        MediumCount=$sevM.Count;
        MediumMaxAge=($sevM.Group | Measure-Object $ageField -Maximum).Maximum;
    })
}

Write-Host "writing summary file to $summaryFileName"
$summaryData | ConvertTo-Csv | Out-File $summaryFileName -Force
Write-Host "done" -ForegroundColor Green
```
