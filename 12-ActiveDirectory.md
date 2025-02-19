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

2) Once Admin on MS01, **don't immediately get jump to MS02**. Enumerate MS01 throughly this time as Admin. There may be secrets that you can find. Also please remember to dump the NTLM hashes and check lsa secrets using mimikatz. You may gain additional domain creds or passwords that you can then use.

3) Once you have enumerated MS01 as Admin and have gained (or not gained) any sensitive information, start a ligolo tunnel so that you can attack MS02 and query the DC. 

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
```

2) Do manual enumeration of MS01 first. This means checking out the following folders:
- C:\Users
- C:\  # check for any non default folders here and investigate them
- C:\inetpub # if there are web services on MS01

If there are anything interesting, check whether you can edit the files or add stuff to these directories. 

You should also do host, user and group enumeration.