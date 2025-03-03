# Determine the SQL Server version

Using a SQL command:

```sql
SELECT @@version
```

Using PowerShell via SqlCmd:

```powershell
Invoke-SqlCmd -ServerInstance SERVER_NAME -Query "SELECT @@Version AS Version" | Select-Object -ExpandProperty Version
```

Example output:

```
Microsoft SQL Server 2022 (RTM-CU15-GDR) (KB5046862) - 16.0.4155.4 (X64)
        Oct 18 2024 16:16:11
        Copyright (C) 2022 Microsoft Corporation
        Express Edition (64-bit) on Windows Server 2022 Standard 10.0 <X64> (Build 20348: ) (Hypervisor)
```
