# Find a VSS volume by its ID

Use the PowerShell cmdlet `Get-CimInstance` to find a `Win32_ShadowCopy` instance with matching ID.

## Examples

### PowerShell snippet

```powershell
Get-CimInstance -Class Win32_ShadowCopy -Filter "ID='{placeholder-guid-value}'"
```

### PowerShell function

This function makes sure the ID is passed to `Get-CimInstance` in the expected format

```powershell
function Get-VssShadowVolume {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [System.Guid]
        $ID
    )
    
    process {
        Get-CimInstance -Class Win32_ShadowCopy -Filter "ID='$($ID.ToString('B'))'"
    }    
}
```
