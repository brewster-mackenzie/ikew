# Prompt the user to select yes/no

Prompt the interactive user to answer 'yes' or 'no' in response to a query.

## PowerShell

This example sets 'yes' as the default answer

```powershell
0 -eq $Host.UI.PromptForChoice('caption', 'prompt text', @('&Yes', '&No'), 0)
```

### Outputs

`$true` if the user answers 'yes'
`$false` if the user answers 'no'


