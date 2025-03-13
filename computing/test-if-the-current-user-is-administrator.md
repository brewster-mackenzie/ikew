# Test if the current user is Administrator

#powershell #security 

-----

## PowerShell

```powershell
using namespace System.Security.Principal
$principal = [WindowsPrincipal][WindowsIdentity]::GetCurrent()
$principal.IsInRole('Administrator')
```

