# Generate a SQL Server login audit report

#mssql #security 

-----

Use PowerShell to generate report of current SQL login mappings and failed logins, which are helpful reports for auditing SQL access.

Set the `$server` variable to the name of your SQL server instance.

```powershell
$f1 = Join-Path $pwd 'SqlLoginMappings.csv'
$f2 = Join-Path $pwd 'SqlFailedLogins.csv'

$server = 'TEST_SQL1'

Invoke-SqlCmd -ServerInstance $server -Query @"
    CREATE TABLE #tmpMSLoginMappings(
        LoginName NVARCHAR(MAX),
        DBName NVARCHAR(MAX),
        UserName NVARCHAR(MAX),
        AliasName NVARCHAR(MAX)
    );

    INSERT INTO #tmpMSLoginMappings
    EXEC master..sp_msloginmappings

    SELECT * FROM #tmpMSLoginMappings
    ORDER BY LoginName, DBName ASC

    DROP TABLE #tmpMSLoginMappings
"@ | ConvertTo-Csv -NoTypeInformation | Out-File $f1 -Force

Write-Host "SQL Login Mappings ---> $f1"

Invoke-SqlCmd -Hostname localhost -Query @"
    EXEC master.dbo.xp_readerrorlog 0, 1, N'login failed', null, NULL, NULL, N'desc'
"@ | ConvertTo-Csv -NoTypeInformation | Out-File $f2 -Force

Write-Host "SQL Failed Logins ---> $f2"
```
