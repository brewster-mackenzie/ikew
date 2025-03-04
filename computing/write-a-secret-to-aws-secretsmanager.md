# Write a secret to AWS SecretsManager

#aws #secretsmanager #powershell #jq

-----

## AWS CLIv2 

To simply create a secret named `$secretName` with value `$secretValue`

```bash
aws secretsmanager create-secret --secret-id $secretName --secret-string $secretValue
```

And to return its ARN using `jq` do:

```bash
aws secretsmanager create-secret --secret-id $secretName --secret-string $secretValue --output json | jq -r '.ARN'
```

## PowerShell 

Use PowerShell to read the ARN from the response data

```powershell 
$arn = aws secretsmanager create-secret --secret-id $secretName --secret-string $secretValue --output json | 
  ConvertFrom-Json | Select-Object -ExpandProperty ARN
```

## Updating 

You can use `update-secret` instead of `create-secret` in the above commands
to update the value of an existing secret.
