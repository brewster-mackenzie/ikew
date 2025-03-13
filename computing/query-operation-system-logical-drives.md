# Query operation system logical drives

#io #filesystem #linux #windows  #bash #powershell

-----

## Linux

### Bash

```bash
# requires lvm2
lvs
```

## Windows

### Command Prompt

```
wmic logicaldisk get deviceid, volumename, description
```

### PowerShell

```powershell
Get-PSDrive -PSProvider FileSystem
```

A more advanced function to get logical drives filtered by the type of drive (e.g. fixed, network, RAM).

```powershell
function Get-LogicalDrives {    
    [CmdletBinding()]
    param(
        [ValidateSet('CDRom', 'Fixed', 'Network', 'NoRootDirectory', 'Ram', 'Removable', 'Unknown')]
        [string[]]
        $DriveType
    )    

    process {
        [System.IO.Directory]::GetLogicalDrives() | Where-Object {
            $null -eq $DriveType -or
            ([System.IO.DriveInfo]$_).DriveType -in $DriveType
        }
    }
}
```

#### Arguments

`DriveType` one or more of CDRom, Fixed, Network, NoRootDirectory, Ram, Removable, Unknown

#### Outputs

an array of `System.IO.Directory` representing the logical volumes matching the filter
