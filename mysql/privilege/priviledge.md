# PRIVILEGES

## Create Database, Create MySQL User and Enable Remote Connections to MySQL Database

This example uses following parameters:

```txt
DB_NAME = webdb
USER_NAME = webdb_user
REMOTE_IP = 10.0.15.25
PASSWORD = password123
PERMISSIONS = ALL
```

```sql
## CREATE DATABASE ##
mysql> CREATE DATABASE webdb;

## CREATE USER ##
mysql> CREATE USER 'webdb_user'@'10.0.15.25' IDENTIFIED BY 'password123';

## GRANT PERMISSIONS ##
mysql> GRANT ALL ON webdb.* TO 'webdb_user'@'10.0.15.25';

##  FLUSH PRIVILEGES, Tell the server to reload the grant tables  ##
mysql> FLUSH PRIVILEGES;
```

