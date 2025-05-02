# Small wl
```bash
feroxbuster -u http://192.168.100.200:8000/ --threads 20 -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt -s 200,300,301
```

# Medium wl
```bash
feroxbuster -u http://192.168.100.200:8000/ --threads 20 -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -s 200,300,301
```
Raft Medium
```bash
feroxbuster -u http://192.168.100.200:8000/ --threads 20 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -s 200,300,301
```
# Big wl
```bash
feroxbuster -u http://192.168.100.200:8000/ --threads 20 -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-big.txt -s 200,300,301
```
