# Install CloudWatch Agent

Amazon Linux:

```bash
sudo yum install amazon-cloudwatch-agent
```

Windows:

```powershell
Invoke-WebRequest https://amazoncloudwatch-agent.s3.amazonaws.com/windows/amd64/latest/amazon-cloudwatch-agent.msi -OutFile ./amazon-cloudwatch-agent.msi

msiexec /i amazon-cloudwatch-agent.msi
```
