# Ligolo-ng

## When do you use ligolo?

- When you need to access a different subnet from your kali machine; For OSCP this means the AD Set where the DC and the 2nd AD machine will be on a different subnet from your kali

- When you want to access an internal service on a host; usually these are services that are not exposed or services like SQL, MSSQL that only allow logins from localhost

## How does it work?

- This is my layman understanding so happy to be corrected. Ligolo-ng creates a tunnel via the pivot machine (or agent) to allow you to access another subnet. Think of it like a VPN. This is unlike Chisel which uses SOCKS.

- You have a proxy which is your kali and your agent which is the pivot machine

- As such, ligolo-ng is my preference for the OSCP because it is just really good and much faster than something like Chisel where you have to use proxychains and everything slows down to a crawl (esp Nmap scans)

- That said, you should still learn Chisel as an alternative tool

## The Most Common Use Case: For Pivoting into Internal Network

![alt text](/Assets/Basic_ligolo.png)

