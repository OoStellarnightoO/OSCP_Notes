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

**xfreerdp**

I am not sure why this is not in pimpmykali but this is personally an essential tool for RDP access into windows boxes. I find it more stable than rdesktop and remmina. Refer to the Service Enumeration .md file for more info on how to xfreerdp to its fullest potential!

```bash
sudo apt install freerdp2-x11
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

**Bloodhound CE**

I am more used to Bloodhound CE instead of the traditional Bloodhound so it is up to you if you want to do this. I believe that traditional Bloodhound is no longer supported so might as well get used to using CE. 

The github page can be found here: https://github.com/SpecterOps/BloodHound

I found the following guide to be straightforward in assisting the install process. PimpMyKali should have installed docker-compose

https://blog.spookysec.net/Deploying-BHCE/ 

```bash
sudo git clone https://github.com/SpecterOps/BloodHound.git /opt/BloodHound
cd /opt/Bloodhound
sudo cp ./examples/docker-compose/* ./

# to set it up

sudo docker-compose up

# note the password upon first set up of Bloodhound CE as you need that to login to the admin portal on localhost:8080. You can change the password after that

```

Once you are in the GUI portal, download the SharpHound package from the Download Collectors link. Unzip it. 

![alt text](/Assets/sharphound_collector.png)

**Terminator**

I mean check this out:

![alt text](/Assets/terminator.gif) 

Bascially you can name your terminal windows and tabs AND save it as a custom layout that you can spawn with a shortcut. 

I have two tabs dedicated to scanning and attacking and a final tab for all my tools.

I have a separate .md page on how to customise it >>

But basically create a folder for all your tooling and then create a tab when spawned, points you directly to your tool folder.

**Git-dumper**

Dumps .git files for you. Good when you discover a .git repo on a HTTP/HTTPS website

https://github.com/arthaud/git-dumper

```bash
pipx install git-dumper
```

## Some tools that I heard are cool but I didn't use it

**AutoRecon**

Basically automates a whole bunch of enumeration for you. I am a fan of doing it my own so I didnt use this. I heard great things about this though.

https://github.com/Tib3rius/AutoRecon

**Penelope**

A Netcat on steroids. Upgrades your shell for you and allow you to background it. Looks really colorful as well. Also seem to be able to function as a pseudo C2.
Unfortunately it kinda screwed my original kali due to some dependencies issues so you might want to take virtual image before trying this so that you can always revert.

https://github.com/brightio/penelope

**OSCP-TunnelMonitor**

The OSCP Exam (and Lab) environment has a reputation for being unstable and finciky so someone on Reddit wrote a script to basically ping and try to establish a TCP connect to a port of choice and will notify you if the connection drops.

https://github.com/captain118/OSCP-TunnelMonitor


-----------------------------------
# Tools

Now that you downloaded all the good stuff up there, time to set up your tools folder.

I created a folder for OSCP and then another folder for Tools. Inside the Tools folder, I further break it down to various categories. Feel free to follow along:

```bash
# on my current working directory which is /home/kali/Desktop

mkdir OSCP Tools

cd Tools

mkdir AD_Enum AD_Attack Chisel Ligolo LinuxEnum LinuxPE WinEnum WinPE Mimikatz WebShells Venom Others

```

## AD_Attack

I dont think the attacks for these tools are in scope for OSCP but I downloaded them when I was doing the Certified Box from HTB so hey no harm to have them here

https://github.com/micahvandeusen/gMSADumper

https://github.com/ShutdownRepo/targetedKerberoast/tree/main

## AD_Enum

Dump the SharpHound tools that you downloaded from BloodhoundCE here

## Chisel

Download the latest versions of the binaries from https://github.com/jpillora/chisel/releases

you want the 

chisel_version_linux_amd64.deb  for linux,  
chisel_version_linux_amd64.deb for Windows 64bit and  
chisel_version_windows_386.gz for Windows 32bit

These should work out of the box. I discuss more how to use this in the pivoting notes.

## Ligolo

Download the latest versions of the binaries from https://github.com/nicocha30/ligolo-ng/releases/

you want the 

ligolo-ng_agent_version_linux_amd64.tar.gz (Agent for Linux box)
ligolo-ng_agent_version_windows_amd64.zip  (Agent for Windows box)
ligolo-ng_proxy_version_linux_amd64.tar.gz  (Proxy that you run on Kali)

## LinuxEnum

You want the following Linux Enumeration scripts:

lse.sh > https://github.com/diego-treitos/linux-smart-enumeration
linpeas.sh > https://github.com/peass-ng/PEASS-ng/releases/ 
pspy32 > https://github.com/DominicBreuker/pspy/releases/tag/v1.2.1
pspy64 > https://github.com/DominicBreuker/pspy/releases/tag/v1.2.1

## LinuxPE

These are common kernel exploits for PE. Might as well compile them ahead of time

```bash

mkdir DirtyPipe PwnKit

```







