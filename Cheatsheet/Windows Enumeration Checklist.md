Windows Enumeration Checklist	(Some powershell 'Get' commands will not work if you are not in an interactive session eg. bind shell)

```cmd
whoami
```

```cmd
set
```

```powershell
ConsoleHost_history
```

```cmd
whoami /priv
```

```cmd
whoami /groups
```

```cmd
net user
```
OR
```powershell
Get-LocalUser
```

```cmd
net localgroup
```
OR
```powershell
Get-LocalGroup
```

```powershell
Get-LocalGroupMember groupname
```

```cmd
net user /domain
```

```cmd
net user dave /domain
```

```cmd
net group /domain
```

```cmd
net group "Domain Admins" /domain
```

```cmd
systeminfo
```

```cmd
ipconfig /all
```

```cmd
route print
```

```cmd
netstat -ano
```

Check installed Programs
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```

```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```

```powershell
Get-Process
```

```powershell
Get-History
```

```powershell
(Get-PSReadlineOption).HistorySavePath
```

# Service Enumeration

services.msc
```powershell
Get-Service 
```

Get-CimInstance :
All Services
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName
```
Running Services
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```
Service StartMode (Manual/Auto etc.)
```powershell
Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like 'mysql'}
```

# Scheduled Tasks
```cmd
schtasks /query	
```
OR 
```powershell
Get-ScheduledTask
```

```cmd
schtasks /query /fo LIST /v
```
>verbose output

# Unquoted Service Paths
```cmd
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """
```

# Service binary ACL (can use Get-ACL)
```cmd
icacls "C:\xampp\apache\bin\httpd.exe"
```

# Powershell enumeration of documents 
(edit path if required)

```powershell
Get-ChildItem -Path C:\Users -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
```

## .txt and .ini
```powershell
Get-ChildItem -Path C:\ -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
```

## .kdbx
```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```

# Security Patches
```powershell
Get-CimInstance -Class win32_quickfixengineering | Where-Object { $_.Description -eq "Security Update" }
```


# Domain
```powershell
powershell -ep bypass
```

```powershell
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```

Get Domain Component
```powershell
([adsi]'').distinguishedName
```

The LDAP path should look like this (eg. PDC)
```
LDAP://DC1.corp.com/DC=corp,DC=com
```
