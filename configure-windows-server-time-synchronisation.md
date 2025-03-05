# Configure Windows Server to use the AWS time synchronisation service

#windows #security #ntp #aws

-----

Time server synchronization is controlled via Group Policy for all domain joined Windows hosts.

This requires the following Group Policy Objects to be created:

1. PDC WMI filter
1. PDC Sync GPO
1. Client Sync GPO


## Create Group Policy Objects

### PDC WMI Filter

The first object is a WMI filter to select any hosts flagged as Primary Domain Controller (PDC). 

The concept of PDC is no longer relevant in a Windows domain, but the same filter still applies for domain controllers that assume this role via FSMO.

- create a filter on root/CIMv2
- set the filter query as follows and then save it

```sql
Select * from Win32_ComputerSystem where DomainRole = 5
```

### PDC Sync GPO

This GPO will set the PDC time provider to the the AWS Time Sync Service


- create a GPO and link it to Domain Controllers
- navigate to `System/Windows Time Service/Time Providers`
- edit the policy `Configure Windows NTP Client` as follows:
  - NtpServer=169.254.169.123,0x9
  - Type=2
  - CrossSiteSyncFlags=2
  - ResolvePeerBackoffMinutes=15 
  - ResolvePeerBackoffMaxTimes= 7
  - SpecialPollInterval=3600 
  - EventLogFlags=3
- ensure the following policies are enabled:
  - `Configure Windows NTP Client`
  - `Enable Windows NTP Client`
  - `Enable Windows NTP Server`
- save the GPO and set its WMI Filter to the filter created in the previous step


### Client Sync GPO

The final object is a rule that will tell the others hosts to use the DC for time sync.

- create a GPO in the root
- edit its permissions to remove "Apply" permissions from Authenticated Users and add "Apply" permissions to Domain Computers.
- navigate to `System/Windows Time Service/Time Providers`
- edit the policy `Configure Windows NTP Client` as follows:
  - NtpServer=domain-controller.mydomain.com,0x9 
  - Type=NT5DS 
  - CrossSiteSyncFlags=2 
  - ResolvePeerBackoffMinutes=15 
  - ResolvePeerBackoffMaxTimes=7 
  - SpecialPollInterval=3600 
  - EventLogFlags=0 
- ensure the following policies are enabled:
  - `Configure Windows NTP Client`
  - `Enable Windows NTP Client`
- save the GPO and ensure it is linked to root 


## Verify the changes

Wait for GPO refresh or run it manually:

```powershell
gpupdate /force
```

Check the PDC time source - it should return the IP address of the AWS time sync service.

```powershell
w32tm /query /source

# 169.254.169.123
```

Check the workstation's time source using the same command, which should output the FQDN of the PDC/DC.


