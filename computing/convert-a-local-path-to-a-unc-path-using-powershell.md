# Convert a local path to a UNC path using PowerShell

#networking #io #filesystem #powershell #samba #cifs

-----

Use a regex find and replace to convert a local path e.g. `C:\Hello\World` to a
Samba/CIFS administrative share path (with the `$` suffix) such as `\\server\C$\Hello\World`. 

Set `$localPath` and `$server` appropriately.

```powershell
$localPath -replace '^([a-z])\:\\(.*)$', "\\$server\`$1`$\`$2"
```
