# Read the contents of an INI file

#ini #powershell 

-----

## PowerShell

Read an INI file and output a hash table representing its contents

```powershell
function Read-IniFile {    
    [CmdletBinding()]
    param(    
        [Parameter(Mandatory, Position=0)]
        [string]
        $Path
    )   
    process {
        $ini = @{}
        switch -regex -file ($Path) {
            "^\s*\[(.+)\]\s*$" {  
                $section = $matches[1]            
                $ini[$section] = @{} 
                continue            
            }
            "^\s*(;.*)\s*$" {
                continue
            }
            "^\s*(.+?)\s*=\s*(.*)\s*$" {
                if ($section) {
                    $ini[$section][$matches[1]] = $matches[2]                
                }
                continue
            }
        }
        $ini
    }
}
```

### Arguments

`Path` the path of the file to read (it can have any file extension)

### Outputs

A hash table of key-value pairs for each section of the file, with each value being another hash table containing key-value pairs for each value within that section.

All values are strings.

### Example

The file input:

```ini
[Section1]
Value1=hello
Value2=world
[Section2]
Value3=1
Value4=2
```

Will output:

```powershell
@{
  Section1=@{
    Value1="hello"
    Value2="world"
  },
  Section2=@{
    Value3="1"
    Value4="2"
  }
}
```



