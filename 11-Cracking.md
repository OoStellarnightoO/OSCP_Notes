# Cracking Passwords

This will be a short one. For OSCP, if your hashcat has been running for more than 15 minutes, then the hash is probably not crackable.

The first thing you should do before going straight to hashcat or JTR is to put your hash(NTLM,MD5,SHA256) in https://crackstation.net/

The website basically has a super comprehensive wordlist and has pre-cracked the hashes for each word in their list so if your hash is there, it returns you the cracked version almost instantaneously.

If it doesnt work then it is hashcat time. If despite your best efforts, hashcat doesnt recognise the format of your hash, try JTR. 

To identify a hash mode for hashcat, you can try using hash-identifier. It doesnt always work though so personally I prefer to just go to hashcat's example hash page and then search for a hash pattern that matches my hash

https://hashcat.net/wiki/doku.php?id=example_hashes

I also always use fasttrack first followed by rockyou.txt. There are passwords that exist in fasttrack that are not in rockyou so you should do both. Fasttrack is a small list and is relatively fast to run.

```bash
sudo hashcat -m <mode> /usr/share/wordlists/seclists/fasttrack.txt hash.txt
```

## Cracking Zip Files

```bash
zip2john <zip file> > zip.hash

john zip.hash -wordlist=rockyou.txt

OR
# -D for dictionary attack and u for unzip
fcrackzip -D -u zip.hash -p rockyou.txt
```

## Cracking kdbx files

```bash
# convert to hashcat format
keepass2john Database.kdbx > keepass.hash
# keepass2john prepends Database: as a string to keepass.hash so in this case as KeePass uses a master password without usernames, remove the Database: string from keepass.hash before trying to crack it

hashcat -m 13400 keepass.hash rockyou.txt
```

## Cracking SAM and SYSTEM hives

```bash
impacket-secretsdump -system SYSTEM -sam SAM local
```
