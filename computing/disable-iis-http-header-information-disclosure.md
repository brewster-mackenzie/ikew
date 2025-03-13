# Disable IIS HTTP Header Information Disclosure

#iis #security #http #powershell

-----

Use PowerShell to remove the following information headers:

- X-AspNet-Version
- X-Powered-By
- Server

```powershell 
# X-AspNet-Version
Set-WebConfigurationProperty -PSPath 'MACHINE/WEBROOT'  -Filter "system.web/httpRuntime" -Name "enableVersionHeader" -Value "False"
# X-Powered-By
Remove-WebConfigurationProperty -PSPath "MACHINE/WEBROOT/APPHOST" -Filter "system.webServer/httpProtocol/customHeaders" -Name . -AtElement @{name='X-Powered-By'}
# Server
Set-WebConfigurationProperty -PSpath 'MACHINE/WEBROOT/APPHOST'  -Filter "system.webServer/security/requestFiltering" -Name "removeServerHeader" -Value "True"
```
