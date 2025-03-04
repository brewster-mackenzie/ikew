# Set file audit ACLs using PowerShell

#security #windows #powershell #filesystem #io

-----

Get the ACL using `Get-Acl` and modify the audit rule with `SetAuditRule` before
setting the new ACL using `Set-Acl`.

Example which audits all write access to a file or folder `$path` and applies
the rule recursively if the target is a folder.


```powershell  
$path = 'C:\some\file-or-folder\path.txt'

$fileAuditRule = New-Object System.Security.AccessControl.FileSystemAuditRule('Everyone', 'Write,Delete,ChangePermissions', 'Success')
$folderAuditRule = New-Object System.Security.AccessControl.FileSystemAuditRule('Everyone', 'Write,Delete,ChangePermissions', 'ContainerInherit,ObjectInherit', 'None', 'Success') 

$item = Get-Item $path
$rule = if ($item.PSIsContainer) { $folderAuditRule } else { $fileAuditRule }
$sacl = Get-Acl -Path $path -Audit
$sacl.SetAuditRule($rule)
$sacl | Set-Acl -Path $path
```
