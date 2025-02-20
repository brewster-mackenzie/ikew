# Assume an IAM role in AWS

It is necessary to assume an IAM role in order to perform role restricted actions in AWS.

This can be performed in the AWS Management Console or with a command issued via the shell.

## AWS CLI v2

```bash
aws sts assume-role --role-arn <value> --role-session-name <value>
```

## PowerShell

```powershell
$RoleARN = '<placeholder>'
$SessionName = '<placeholder>'
$Role = Use-STSRole -RoleArn $RoleARN -RoleSessionName $SessionName
Set-AWSCredential -Credential $Role.Credentials -Scope Global
```

