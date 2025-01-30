# Introduction

The OSCP may be 23 hours 45 minutes but you have a lot longer than that to prepare your environment for the exam. Proper Preparation is half of the battle for OSCP. The last thing you want is to scramble for working tools on the exam itself.

For me, my trusty Kali VM decided to throw tantrum regarding my netexec dependencies after I installed penelope to give it a try.

So the day before my OSCP, I prepared a completely fresh Kali VM. This is how I did it and what worked for me.

Feel free to modify to your own needs. I also added some videos that I thought were useful.

-------------------------------------------------------------

## Steps

Now you have installed your Kali. What's next?


### PimpMyKali

First of all, I would like to introduce this wonderful script by Dewalt that would help install 95% of the tools you need for OSCP:

This includes things like nxc, seclists (i am surprised that this does not come native with kali), enum4linux-ng etc

https://github.com/Dewalt-arch/pimpmykali

Do this first. Read more here: https://tcm-sec.com/pimpmykali-pmk-v-2-0-release/

_That aside, I am always an advocate for TCM Security's affordable courses so give them a look if you wish!_

```bash
# Remove existing pimpmykali folder
rm -rf pimpmykali/

# Clone pimpmykali repository & enter the folder
git clone https://github.com/Dewalt-arch/pimpmykali

cd pimpmykali

# Execute the script - For a new Kali VM, run menu option 'N'
# (The script must be run with root privileges)
sudo ./pimpmykali.sh
```

nxc needs to be initialised upon first use so just run the below command

```bash
nxc smb 127.0.0.1 -u '' -p ''
```

### Installing Other Tools

**Feroxbuster**

A directory busting tool that does recursive busting. Useful tool to have in addition to Gobuster. Also used in Tiberius's famous AutoRecon (though I did not use AutoRecon for OSCP). More details on usage in the HTTP Enumeration .md file

```bash
sudo apt install feroxbuster
```

**Sublime Text**

I love this text editor. I am not haxor enough to use vim or nano to their full potential. Sublime Text is a noob friendly way to edit text which you will be doing a lot of from things like processing domain usernames from a nxc smb --users dump to looking at exploit code. You can easily replace all whitespaces with the find and replace function and there are multiple plugins. 

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text

# after installing

subl # to open sublime text
```

**rlwrap**

Don't you hate it when your nc reverse shell cant access previous commands by pressing up or you cant edit your commands by pressing left?
Now you can! Look at the github page for more info : https://github.com/hanslub42/rlwrap

```bash
sudo apt -y install rlwrap

# Usage

rlwrap nc -lvnp 4444
```

**Keepass**

Application to open keepass database files (.kdbx). You will encounter such files when doing your OSCP challenge labs so best to get it ready.

```bash
sudo apt install keepassxc
```

**snmp-mibs-downloader**

Crucial when confronting SNMP UDP port. I will explain more in the Service Enumeration part. For now, just install it.
For more reading, refer to this: https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-snmp/index.html#enumerating-snmp

```bash
sudo apt-get install snmp-mibs-downloader
sudo download-mibs
sudo nano /etc/snmp/snmp.conf # comment out mibs
```