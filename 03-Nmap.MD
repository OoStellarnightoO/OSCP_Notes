# Nmap

The most critical command in OSCP. Do it good and do it well.

Here is what I use (with explanations):

```bash
# running nmap on Sudo allows nmap to run a TCP Syn scan which is faster and more stealthier than a unprivileged nmap scan
# -p- tells nmap to scan all ports which in this case are the 65535 TCP Ports. You must scan all ports because there may be services sitting on non-default ports
# -A tells nmap to run  OS detection, version detection, script scanning, and traceroute
# -Pn tells nmap to skip host discovery which is done through ICMP. In OSCP, you know the host is up at the target IP. If it is a windows box, nmap may fail if this flag is not provided because Windows block ICMP by default thereby making nmap think the host is down. So always use -Pn for OSCP
# -v for verbose. I just like to see the results live so that I can start enumerating even when the nmap scan is ongoing
# --open tells nmap to only run the version detection, script enumeration against ports that are open. This is so that nmap doesnt take forever to scan as it will run -A against every port without this flag!

sudo nmap -p- -A -Pn -v --open <target ip> -oN <output file>

# if you get some kind of CN or hostname from the scans try running the above again but instead of the target ip, scan the hostname

sudo nano /etc/hosts # add the hostname to the target ip

sudo nmap -p- -A -Pn -v --open <hostname> -oN <output file>

```

You should run a UDP scan as well just in case as sometimes SNMP shows up and that will usually be the attack vector. 
UDP scan need to be run with Sudo
I only use the -top-ports flag as UDP scan is connectionless and will TAKE FOREVER if you scan everything. 50 ports are good enough

```bash
sudo nmap -sU -top-ports=50 -A -Pn -v --open <target ip> --reasons
```

If you run into an odd service that when connected to pumps out strange strings, try piping the output and then running the file command

```bash

nc <ip> <strange port> > test.txt

file test.txt 
```