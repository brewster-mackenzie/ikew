# Create a random temporary folder

## PowerShell

```powershell
function New-TemporaryFolder {
  [CmdletBinding()]
  [Alias('New-TemporaryDirectory')]
  [Alias('New-TempFolder')]
  [Alias('New-TempDirectory')]
  param ()
  process {
    $name = (1..10 | % { $r = [Random]::new()} { [char]$r.Next(65, 90)}) -join ''
    New-Item -ItemType Directory (Join-Path $env:TEMP $name)
  }
}
```



