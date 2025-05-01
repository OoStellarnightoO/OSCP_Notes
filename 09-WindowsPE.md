# Windows PrivEsc

## SeImpersonatePrivilege

If you found a foothold via a web application, chances are that the corresponding user will have SeImpersonatePrivilege. This is because this permission is usually assigned to web and database services.

If you see this privilege, 3 methods of privilege escalation come to mind - SigmaPotato, GodPotato and PrintSpoofer.

The reason why 2 Potato exploits are stated here is because of potential compatibility issues with Windows OS versions and/or .NET framework.
Generally, you would want to attempt to use SigmaPotato first as it is newer. I personally prefer going in this order : 
SigmaPotato > PrintSpoofer > GodPotato

If everything goes well (it should), you will find yourself elevated to NT AUTHORITY\SYSTEM
## Hijacking

Includes but is not limited to :
Executable files
DLLs
Powershell Scripts
Batch Scripts

Scripts can mostly be written on your own, while compiling or creating the DLLs and executable files are usually done via msfvenom at this point. 

Alternatively you could write your own payload in C/C++ and cross compile it yourself if that tickles your fancy ;D
### Scheduled Tasks

Scheduled Tasks can be enumerated via the GUI if you have access to the machine or with
```powershell
schtasks /query	
Get-ScheduledTask
schtasks /query /fo LIST /v
```

If you spot a task that could potentially lead to privilege escalation, you can navigate to the path where it executes the binary/script and attempt to hijack it.

You can check for ACLs with the `icacls` command eg.
```powershell
# you can run it on folders/binaries alike
icacls "C:\xampp\apache\bin\httpd.exe"
```

If the user has SeShutdownPrivilege, there is a chance that you might be required to restart the machine in order to run your payload. This is sometimes required depending on the task's frequency of execution. 
### Services

Similarly to scheduled tasks, services can be enumerated via :
```powershell
# all services
Get-CimInstance -ClassName win32_service | Select Name,State,PathName

# running services
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```

And unquoted service paths via :
```cmd
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """
```

If a particular service is deemed to be a privilege escalation vector, you can then check for ACL permissions with `icacls`, similar to what was mentioned earlier in the scheduled tasks section.
It helps with identifying which folders are writable if it is possible to abuse unquoted service paths.

## Miscellaneous

Don't forget to check the Users folder as there may be credentials or sensitive documents/files leading to privilege escalation in a different manner.