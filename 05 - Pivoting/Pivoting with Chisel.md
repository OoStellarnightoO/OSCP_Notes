# Chisel

```bash
# on Kali

chisel server --socks5 --reverse

# make sure that the /etc/proxychains4.conf final line is:
socks5 127.0.0.1 1080

```

On the pivot (Windows)

```cmd
# grab the fingerprint from the chisel server on kali when it starts

chisel.exe client --fingerprint W4fR5CxHtK6hB6P0KsDfsMPxmTBqEJ8cD+2ml17Z5jY= 192.168.45.152:8080 R:socks

```

You need to run all commands with proxychains in front!

To browse websites internal

use foxyproxy and create a new proxy

![alt text](/Assets/chisel.png)

then switch it on and then key in the internal ip address to browse

Also just watch John Hammond's great video: https://www.youtube.com/watch?v=pbR_BNSOaMk