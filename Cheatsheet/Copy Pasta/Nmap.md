# Open ports
```bash
sudo nmap -p- -n -Pn --open 192.168.100.200 -oN target_ports
```

# Services and OS (-sCV -O)

Single Target
```bash
sudo nmap -p- -n -Pn -sCV -O 192.168.100.200 -oN target_detailed
```

List
```bash
sudo nmap -p- -n -Pn -sCV -O -iL iplist -oN target_detailed
```

--------------------------------------------------------------
# UDP (top ports)
```bash
sudo nmap -sU --top-ports=100 -Pn -n 192.168.100.200 -oN target_udp
```

# UDP (if time permits)
```bash
sudo nmap -sU -p- -sCV -Pn -n 192.168.100.200 -oN target_udp_full
```

# List
## Ports
```bash
sudo nmap -p- -n -Pn --open -vv -iL iplist -oN target_ports
```

## Services
```bash
sudo nmap -p- -n -Pn -sCV -O -iL iplist -oN target_detailed
```

## UDP
```bash
sudo nmap -sU --top-ports=100 -Pn -n -iL iplist -oN target_udp
```

## SNMP detailed
```bash
sudo nmap -sU -sCV -p 161 -Pn -n iplist -oN target_snmp_detailed
```