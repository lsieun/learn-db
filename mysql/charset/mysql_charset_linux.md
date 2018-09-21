# Linux MySQL 5.7 修改默认字符集为utf8

URL:

- https://blog.csdn.net/jack85986370/article/details/51454886

[TOC]

## 1. 查看mysql字符集情况

```sql
mysql> SHOW VARIABLES LIKE 'character_set_%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```

其中，`latin1`是很多语言乱码的主要原因，通过修改`my.cnf`的方法，一劳永逸的解决乱码问题。

## 2. 修改默认字符集为utf8

如果不知道`my.cnf`文件在哪里，可以使用`whereis my.cnf`命令查找。 

两处修改的地方

```txt
# 修改处1：添加以下2行
[client]
default-character-set=utf8

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
symbolic-links=0
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

# 修改处2：添加以下3行
default-storage-engine=INNODB
character-set-server=utf8
collation-server=utf8_general_ci
```

## 3. 重启MySQL，验证字符集

重启MySQL服务，命令如下：

```bash
sudo service mysqld restart
```

查看MySQL的字符集，SQL语句如下：

```sql
mysql> SHOW VARIABLES LIKE 'character_set_%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)

```

