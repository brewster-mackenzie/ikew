# Convert a PowerShell SecureString to plain text

#powershell #security #cryptography

-----

```powershell 
# Read user input as SecureString
$lineIn = Read-Host 'Enter your password' -AsSecureString

# Convert the SecureString to plain text 
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($lineIn)
$text = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
[Runtime.InteropServices.Marshal]::ZeroFreeBSTR($bstr)

# Write the plain text value
Write-Host $text
```
