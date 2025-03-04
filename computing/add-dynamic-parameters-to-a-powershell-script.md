# Add dynamic parameters to a PowerShell script

#powershell 

-----

To add dynamic parameters to a PowerShell script, make use of the `DynamicParam`
block.  You can combine `param()` with `DynamicParam` if required.


```powershell 
DynamicParam {        
  # Create a dynamic parameter 'DynValue' which accepts inputs
  # from the list of values contained in the file 'dynamic-param.txt'

  # 1. Read the list of values
  $dynamicSet = Get-Content "dynamic-params.txt"

  # 2. Create a Parameter attribute for the new parameter
  $dynValueAttribute = New-Object System.Management.Automation.ParameterAttribute
  $dynValueAttribute.Position = 0
  $dynValueAttribute.Mandatory = $true
  $dynValueAttribute.HelpMessage = "Provide a value from the dynamic set"        

  # 3. Create a ValidateSet attribute for the new parameter 
  $validateAttribute = New-Object System.Management.Automation.ValidateSetAttribute $dynamicSet 

  # 4. Create an attribute collection for the new parameter, 
  # including the Parameter and ValidateSet attributes we created 
  $dynValueAttributeCollection = New-Object System.Collections.ObjectModel.Collection[System.Attribute]
  $dynValueAttributeCollection.Add($dynValueAttribute)
  $dynValueAttributeCollection.Add($validateAttribute)

  # 5. Create the parameter 
  $dynValueParam = New-Object System.Management.Automation.RuntimeDefinedParameter('DynValue', [string[]], $dynValueAttributeCollection)
  
  # 6. Return a parameter dictionary containing the new parameter 
  # We can add mode parameters if we want to.
  $paramDictionary = New-Object System.Management.Automation.RuntimeDefinedParameterDictionary
  $paramDictionary.Add('Target', $targetParam)
  return $paramDictionary        
}
```
