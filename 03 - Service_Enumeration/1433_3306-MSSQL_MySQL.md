# MSSQL and MySql

- Come back later when you see this because this dont usually allow remote logins. 

- that said, it is good to note down that there is SQL so that when you get a foothold, you can then enumerate the SQL service locally.

- You usually see MySQL or MSSQL when you see a webserver with a login function. So if you see that, you might want to try SQL injections on the web services instead.

## Basic Commands

```bash
# For MySQL
mysql -h <Target ip> -u '' -p

# for MSSQL
impacket-mssqlclient Administrator:Lab123@<Target ip> -windows-auth
```