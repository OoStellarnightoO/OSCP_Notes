# Squid Proxy

We can use this tool to scan for internal ports

https://github.com/aancw/spose/blob/master/spose.py

```bash

python spose.py --proxy http://<target ip>:3128 --target <target ip>
```

If you see Squid proxy. do the following to access the internal ports

```bash

sudo nano /etc/proxychains4.conf

# Add the following line to the final line

http <target ip> <port>

# after that you can run a simple nmap to see which ports are open internally on the target
proxychains nmap -sT -n -p- <target ip>
# You can also use the foxyproxy to create a proxy to allow you to access web servers internally!

# foxyproxy is a firefox extension that you can download from its extension page

# if you want to be OG, you can use the in-built proxy setting that comes built-in with firefox
```