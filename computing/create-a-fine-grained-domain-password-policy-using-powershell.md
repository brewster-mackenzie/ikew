# Create a fine grained domain password policy using PowerShell

#windows #security #powershell 

-----

Invoke the following command in PowerShell (adjust parameters as required).

```powershell
$policyParams = @{
    Name = "PasswordPolicy"
    ComplexityEnabled = $true
    LockoutDuration = "00:30:00"
    LockoutObservationWindow = "00:30:00"
    LockoutThreshold = "0"
    MaxPasswordAge = "30.00:00:00"
    MinPasswordAge = "1.00:00:00"
    MinPasswordLength = "16"
    PasswordHistoryCount = "24"
    Precedence = "1"
    ReversibleEncryptionEnabled = $false
    ProtectedFromAccidentalDeletion = $true
}

New-ADFineGrainedPasswordPolicy @policyParams
```

And apply it to a domain entity with:

```powershell 
Add-ADFineGrainedPasswordPolicySubject PasswordPolicy -Subjects groupA
```
