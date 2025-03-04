# Get ports currently in use

#powershell #networking #netstat #ss

-----

Commands to get a list of ports that are currently in use.

## Windows 

Using PowerShell

```powershell  
$usedPorts = (Get-NetTCPConnection | Select-Object -ExpandProperty LocalPort) + 
             (Get-NetUDPEndpoint | Select-Object -ExpandProperty LocalPort)
```

## Linux

### `ss`

```bash
ss -lntup
```

- `-l` to get listening ports 
- `-n` to get port number without resolving service name
- `-t` to include TCP 
- `-u` to include UDP 
- `-p` to include the program name


### `netstat`

> [!warning] Deprecated
> This approach is deprecated in favour of using `ss` 

```bash 
netstat -lntup
```

- `-l` to get listening ports 
- `-n` to get port number without resolving service name
- `-t` to include TCP 
- `-u` to include UDP 
- `-p` to include the program name 



