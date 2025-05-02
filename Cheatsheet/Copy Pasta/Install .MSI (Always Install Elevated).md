If both registry keys are 0x1 (enabled)
```powershell
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1
    
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1
```

```powershell
msiexec.exe /i "C:\ProgramData\reverse_443.msi" /quiet
```
>Source : https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec

/i for normal installation
/a for administrative installation
/x to uninstall