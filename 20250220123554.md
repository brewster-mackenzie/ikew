# Compute the hash value of a string

Snippets to assist with computing the hash value of a string.

## PowerShell

Computing the hash value of a string using PowerShell requires the following steps:

1. convert the string into a UTF-8 encoded byte array
1. initialize the hash algorithm and invoke it against the byte array
1. format the byte array as a hexadecimal string

### Example snippet

```powershell
# Initialize an instance of hash algorithm. 
# Replace <value> with the algorithm name, e.g. MD5, SHA256.
$algorithm = [System.Security.Cryptography.HashAlgorithm]::Create('<value>')

# Convert the string '<value>' into an array of bytes
$bytes = [System.Text.Encoding]::UTF8.GetBytes('<value>')

# Compute hash for the array of bytes
$hashedBytes = $algorithm.ComputeHash($bytes)

# Convert array of bytes into an array of hexadecimal strings
$hashedStrings = $hashedBytes | ForEach-Object { $_.ToString('x2') }

# Combine the array of strings
$hashedStrings -join ''
```

### Example function 

```powershell
function Get-Hash {
	[CmdletBinding(DefaultParameterSetName='Default')]    
	param(
		[Parameter(Mandatory, ValueFromPipeline, Position=0)]
		[string]
		$Value,
		
		[Parameter(Mandatory, Position=1)]
		[string]
		$Algorithm,

		[Parameter(ParameterSetName='UpperCase')]
		[switch]
		$UpperCase
	)
	process {
		$f = if ($UpperCase) { 'X2' } else { 'x2' }				
		$a = [System.Security.Cryptography.HashAlgorithm]::Create($Algorithm) 
		$b = [System.Text.Encoding]::UTF8.GetBytes($Value)
		$h = $a.ComputeHash($b)
		$s = $h | ForEach-Object { 
			$_.ToString($f)
		}
		$s -join ''
	}
}
```
