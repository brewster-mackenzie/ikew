# Force an MS SQL database offline

#sql #mssql

-----

Force rollback, put the database into single-user mode and take it offline

Replace `$catalog` with the catalog name.

```sql
ALTER DATABASE [$catalog] SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
ALTER DATABASE [$catalog] SET OFFLINE;
```

Bring it back online using the approach detailed [[bring-an-ms-sql-database-back-online|here]]
