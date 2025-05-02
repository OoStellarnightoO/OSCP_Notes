# Squid Proxy spose script
```bash
for ip in $(cat /usr/share/seclists/Discovery/Infrastructure/common-http-ports.txt); do python spose.py --proxy http://192.168.100.200:3128 --target 192.168.100.200 --ports $ip; done
```