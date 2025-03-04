# Create a bacpac export an MS SQL database

#sql #mssql #backup #export 

-----

Export/backup a MS SQL Server database using SqlPackage.exe 

Set the `$server`, `$catalog` and `$bacpacFile` variables then run the command:

```powershell
./SqlPackage.exe /a:export /scs:"Server=$server; Database=$catalog; Trusted_Connection=True; TrustServerCertificate=True" /tf:"$bacpacFile"
```
