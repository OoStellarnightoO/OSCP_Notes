# SMTP (Port 25)/ IMAP (Port 143)

if you see this, there may be an exploit related to client side attacks so keep this in mind. 

Also once you get an initial foothold, be sure to check the /var/mail folder. Might have mail.

You can also try to enumerate users either manually or through tools

```bash
Usage: smtp-user-enum [options] ( -u username | -U file-of-usernames ) ( -t host | -T file-of-targets )

options are:
        -m n     Maximum number of processes (default: 5)
	-M mode  Method to use for username guessing EXPN, VRFY or RCPT (default: VRFY)
	-u user  Check if user exists on remote system
	-f addr  MAIL FROM email address.  Used only in "RCPT TO" mode (default: user@example.com)
        -D dom   Domain to append to supplied user list to make email addresses (Default: none)
                 Use this option when you want to guess valid email addresses instead of just usernames
                 e.g. "-D example.com" would guess foo@example.com, bar@example.com, etc.  Instead of 
                      simply the usernames foo and bar.
	-U file  File of usernames to check via smtp service
	-t host  Server host running smtp service
	-T file  File of hostnames running the smtp service
	-p port  TCP port on which smtp service runs (default: 25)
	-d       Debugging output
	-w n     Wait a maximum of n seconds for reply (default: 5)
	-v       Verbose

Examples:

$ smtp-user-enum -M VRFY -U users.txt -t 10.0.0.1
$ smtp-user-enum -M EXPN -u admin1 -t 10.0.0.1
$ smtp-user-enum -M RCPT -U users.txt -T mail-server-ips.txt
$ smtp-user-enum -M EXPN -D example.com -U users.txt -t 10.0.0.1
```

You can use swaks to send email or use thunderbird for a GUI

```bash

# prep the config.Library-ms and body.txt in advance

sudo swaks -t <target email> --from <legit email> --attach @config.Library-ms --server <target ip> --body @body.txt --header <anything you want> --suppress-data -ap
