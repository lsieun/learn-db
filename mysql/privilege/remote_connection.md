# 允许MySQL远程访问

[TOC]

允许mysql远程访问,可以使用以下三种方式:

- 改表（修改mysql.user表的数据）
- 授权（使用MySQL命令）
- 在安装MySQL的机器上运行（言外之意，以上两种方式可以在远程机器上使用）

## 1. 改表

```sql
mysql -u root –p
mysql>use mysql;
mysql>update user set host = '%' where user = 'root';
mysql>select host, user from user;
```

## 2. 授权

例如，你想root使用123456从任何主机连接到mysql服务器。

```sql
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
```

如果你想允许用户jack从ip为10.10.50.127的主机连接到mysql服务器，并使用654321作为密码

```sql
mysql>GRANT ALL PRIVILEGES ON *.* TO 'jack'@’10.10.50.127’ IDENTIFIED BY '654321' WITH GRANT OPTION;
mysql>FLUSH RIVILEGES
```

## 3. 在安装MySQL的机器上运行

```sql
# 进入MySQL服务器
d:\mysql\bin\>mysql -h localhost -u root
# 赋予任何主机访问数据的权限
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION
//使修改生效
mysql>FLUSH PRIVILEGES
//退出MySQL服务器
mysql>EXIT
```

