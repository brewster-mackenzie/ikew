# Test if the current user is Administrator

## PowerShell

```powershell
using namespace System.Security.Principal
$principal = [WindowsPrincipal][WindowsIdentity]::GetCurrent()
$principal.IsInRole('Administrator')
```

