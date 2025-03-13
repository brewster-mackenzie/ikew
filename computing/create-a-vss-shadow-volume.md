# Create a VSS shadow volume

#vss #io #backup #powershell 

-----

Create a VSS shadow volume by invoking the `Create` method on the Win32_ShadowCopy class, with these arguments:

`DriveName` the drive for which the shadow volume should be created, e.g. `c:\`

`Context` specifies how the shadow volume will be created, queried an deleted

## Examples

### PowerShell snippet

> [!info]
> requires an elevated session

```powershell
([WMICLASS]'Win32_ShadowCopy')Create('c:\', 'ClientAccessible')
```

### PowerShell function with error handling

> [!info]
> requires an elevated session

```powershell
function New-VssShadowVolume {       
    [CmdletBinding()]
    param(
        [Parameter(Mandatory, ValueFromPipeline)]                
        [System.IO.DriveInfo]
        $Drive,
        [Parameter()]
        [ValidateScript({$_ -in ([WMICLASS]'Win32_ShadowContext').Properties.Name})]
        [string]
        $Context = 'ClientAccessible'
    )
    
	  process {    
        Write-Verbose "Creating new VSS shadow volume of drive $Drive"

				$class = [WMICLASS]'Win32_ShadowCopy'                
				$result = $class.Create($Drive.Name, 'ClientAccessible')            

				switch($result.ReturnValue) {                    
						0 { Get-CimInstance Win32_ShadowCopy -Filter "ID='$($result.ShadowID)'"}
						1 { throw 'Access denied.'; }
						2 { throw 'Invalid argument.'; }
						3 { throw 'Specified volume not found.'; }
						4 { throw 'Specified volume not supported.'; }
						5 { throw 'Unsupported shadow copy context.'; }
						6 { throw 'Insufficient storage.'; }
						7 { throw 'Volume is in use.'; }
						8 { throw 'Maximum number of shadow copies reached.'; }
						9 { throw 'Another shadow copy operation is already in progress.'; }
						10 { throw 'Shadow copy provider vetoed the operation.'; }
						11 { throw 'Shadow copy provider not registered.'; }
						12 { throw 'Shadow copy provider failure.'; }
						else { throw 'Unknown error.'; }            
				}     
		}   		
}
```
