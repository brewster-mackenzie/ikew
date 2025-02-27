# Generate a report of Active Directory administrators

Modify the list of `$groups` in this PowerShell script, then run it and check the output in `AD-Admins-Report.csv`

```powershell
# Recursive report of administrative group members

# Report dates should be in CSV-friendly format
$dateFormat = 'yyyy-MM-dd'

# Set list of admin groups to check
$groups = 'Administrators', 'Domain Admins', 'AWS Delegated Administrators'

# Get details of admin group members (recursive)
$adminUsers = foreach ($group in $groups) {
    $members = try {
        Get-ADGroupMember $group -Recursive -ErrorAction Ignore 
    }
    catch { $null }

    $users = $members | Where-Object objectClass -EQ user 
    foreach ($user in $users) {
        Get-ADUser -Identity $user.DistinguishedName -Properties created, enabled, lastLogon, memberOf, passwordLastSet
    }
} 

# Get details we care about; format the dates & membership lists
$adminUsers = $adminUsers | Select-Object -Unique DistinguishedName, SamAccountName, UserPrincipalName, Name, Enabled, 
@{N = 'Created'; E = { $_.Created.ToString($dateFormat) } },
@{N = 'LastLogon'; E = { [DateTime]::FromFileTime($_.LastLogon).ToString($dateFormat) } },
@{N = 'PasswordLastSet'; E = { $_.PasswordLastSet.ToString($dateFormat) } },
@{N = 'MemberOf'; E = { $_.MemberOf -join '; ' } } 

# Write to file
$adminUsers | ConvertTo-Csv -NoTypeInformation | Set-Content "AD-Admins-Report.csv"
```
