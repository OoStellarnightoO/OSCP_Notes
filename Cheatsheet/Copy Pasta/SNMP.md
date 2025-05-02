Resource
https://github.com/TheSnowWight/hackdocs/blob/master/pentesting/pentesting-snmp/README.md

# If not already installed 
```bash
sudo apt-get install snmp-mibs-downloader
```

```bash
snmpwalk -v 1 -c public 192.168.100.200 NET-SNMP-EXTEND-MIB::nsExtendObjects
```

```bash
snmpwalk -v 1 -c public 192.168.100.200 NET-SNMP-EXTEND-MIB::nsExtendOutputFull
```

Brute force community strings
```bash
hydra -P /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt 192.168.100.200 snmp
```
