# Mount a VSS shadow volume

#vss #filesystem #backup #powershell 

-----

Mounting a VSS shadow volume to a folder in the local filesystem requires the following steps

1. [[find-a-vss-volume-by-its-id|Find the VSS volume]] based on the volume ID
2. [[create-a-symbolic-link|Create a symbolic link]] to the volume's `DeviceObject` in order to access the volume

## Examples

### PowerShell function 

Provided is an example PowerShell function to mount a VSS shadow volume given its unique ID.

#### Requirements

This example uses the function [[find-a-vss-volume-by-its-id|Get-VssShadowVolume]]

#### Arguments

`ID` the shadow volume ID (GUID)

#### Outputs

the mount point directory

```powershell
function Mount-VssShadowVolume {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory, ValueFromPipeline, ValueFromPipelineByPropertyName)]      
        [System.Guid]
        $ID  
    )  
    
    process {        
		# Create root folder or VSS mount points
		$mntRoot = Join-Path $env:TEMP 'vss-mount'
		if (!(Test-Path $mntRoot)) {
			New-Item $mntRoot -ItemType Directory | Out-Null
		}

		# Locate the volume
        Write-Verbose "Mounting VSS shadow volume $ID"
        $shadow = Get-VSSShadowVolume -ID $ID
        if (!$shadow) {
            throw "Shadow copy with ID '$ID' does not exist"
        }       

		# Delete an existing mount point
        $dir = Join-Path $mntRoot $ID.ToString('B')        
        if (Test-Path $dir) {
            Remove-Item $dir -Force -Recurse | Out-Null
        }

		# Mount the volume
        cmd /c mklink /D "`"$dir`"" "`"$($shadow.DeviceObject)`"" | Out-Null

		# Return the mount point
        Get-Item $dir
    }
}
```




