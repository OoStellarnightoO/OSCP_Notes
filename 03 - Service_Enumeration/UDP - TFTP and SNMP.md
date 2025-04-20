#  TFTP and SNMP


## 69 - TFTP
```bash

sudo nmap -n -Pn -sU -oN tftp.txt -p 69 -sV --script tftp-enum $IP

```

## 161 - SNMP

```bash
# identify the community string first (can use the seclists list)
onesixtyone -c community -i ips

# run both command
snmpwalk -v 2c -c public <IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull

snmpwalk -v 2c -c public <IP> NET-SNMP-EXTEND-MIB::nsExtendObjects

# if the above nsExtend commands do not produce readable output, try running
sudo apt-get install snmp-mibs-downloader
```

If you require to brute force the community string, you can also use hydra

```bash
hydra -P /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt <ip> snmp
```