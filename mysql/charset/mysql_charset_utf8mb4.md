# MySQL使用utf8mb4

URL:

- http://seanlook.com/2016/10/23/mysql-utf8mb4/

[TOC]

## 1. utf8 与 utf8mb4 异同

官方手册 https://dev.mysql.com/doc/refman/5.6/en/charset-unicode-utf8mb4.html 的说明：

> The character set named `utf8` uses a maximum of **three bytes per character** and contains only BMP characters. The `utf8mb4` character set uses **a maximum of four bytes per character** supports supplementary characters:  
> - For **a BMP character**, utf8 and utf8mb4 have identical storage characteristics: same code values, same encoding, same length.
> - For **a supplementary character**, `utf8` cannot store the character at all, whereas `utf8mb4` requires **four bytes** to store it. Because utf8 cannot store the character at all, you have no supplementary characters in utf8 columns and need not worry about converting characters or losing data when upgrading utf8 data from older versions of MySQL.


MySQL在 5.5.3 之后增加了 `utf8mb4` 字符编码，`mb4`即 `most bytes 4`。简单说，`utf8mb4`是 `utf8` 的超集并完全兼容`utf8`，能够用四个字节存储更多的字符。

但抛开数据库，标准的 UTF-8 字符集编码是可以用 1~4 个字节去编码21位字符，这几乎包含了是世界上所有能看见的语言了。然而在MySQL里实现的utf8最长使用3个字节，也就是只支持到了 Unicode 中的 基本多文本平面（U+0000至U+FFFF），包含了控制符、拉丁文，中、日、韩等绝大多数国际字符，但并不是所有，最常见的就算现在手机端常用的表情字符 emoji和一些不常用的汉字，如 “墅” ，这些需要四个字节才能编码出来。

也就是当你的数据库里要求能够存入这些表情或宽字符时，可以把**字段**定义为 `utf8mb4`，同时要注意**连接字符集**也要设置为utf8mb4，否则在 **严格模式** 下会出现 `Incorrect string value: /xF0/xA1/x8B/xBE/xE5/xA2… for column 'name'`这样的错误，非严格模式下此后的数据会被截断。

## 2. utf8mb4_unicode_ci 与 utf8mb4_general_ci 如何选择

字符除了需要存储，还需要**排序**或比较大小，涉及到与编码字符集对应的 排序字符集（collation）。`ut8mb4`对应的排序字符集常用的有 `utf8mb4_unicode_ci`、`utf8mb4_general_ci`，到底采用哪个在 stackoverflow 上有个讨论，[What’s the difference between utf8_general_ci and utf8_unicode_ci](http://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci)

在文中，作者推荐使用`utf8mb4_unicode_ci`。

## 3. 怎么从utf8转换为utf8mb4

一旦你决定使用utf8mb4，强烈建议你要修改服务端 `character-set-server=utf8mb4`，不同的语言对它的处理方法不一样，c++, php, python可以设置`character-set`，但java驱动依赖于 `character-set-server` 选项，后面有介绍。

### 3.1 修改`my.cnf`文件

修改`my.cnf`文件，可以使用`whereis my.cnf`命令查找。 

```txt
[client]
default-character-set = utf8mb4
[mysql]
default-character-set = utf8mb4
[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

### 3.2 验证修改后的字符集

重启数据库，检查变量

```bash
sudo systemctl restart mysqld
```

查看MySQL字符集和字符集的排序信息：

```sql
mysql> SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8mb4                    |
| character_set_connection | utf8mb4                    |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8mb4                    |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
| collation_connection     | utf8mb4_general_ci         |
| collation_database       | utf8mb4_unicode_ci         |
| collation_server         | utf8mb4_unicode_ci         |
+--------------------------+----------------------------+
11 rows in set (0.01 sec)

```


## 4. Java驱动使用

从理解上来讲，`UTF-8`是`UNICODE`的一种**变长字符编码**。所谓“变长字符编码”，就是`UTF-8`用1到6个字节编码UNICODE字符。换句话说，一个字符经过`UTF-8`编码之后，它可以是是1个字节、2个字节、3个字节、4个字节、5个字节、6个字节。

但是，MySQL在实现`UTF-8`编码的时候只支持3字节的数据，而移动端的表情数据(emoji)是4个字节的字符。如果直接表情数据(emoji)添加到`UTF-8`编码的MySQL数据库中，Java程序中将报SQL异常：

```txt
java.sql.SQLException: Incorrect string value: ‘\xF0\x9F\x92\x94’ for column ‘name’ at row 1 
at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:1073) 
at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3593) 
at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3525) 
at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:1986) 
at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2140) 
at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2620) 
at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1662) 
at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1581)
```

如果从MySQL读写`emoji`，需要注意以下三件事情：

- 支持utf8mb4的最低MySQL版本支持版本为5.5.3+，若不是，请升级到较新版本。
- **MySQL驱动版本**要在 `5.1.13` 及以上版本
- **数据库连接**依然是 `characterEncoding=UTF-8` 。

> 注：utf8mb4只是MySQL中的概念，因为MySQL的UTF-8并不是真正意义上的UTF-8。Java语言里面所实现的UTF-8编码就是支持4字节的，所以不需要配置`mb4`这样的字眼，我们的应用中只要使用UTF-8就可以了。

> 下面这段，我没有看懂。

但还没完，遇到一个大坑。[官方手册](http://dev.mysql.com/doc/relnotes/connector-j/5.1/en/news-5-1-13.html) 里还有这么一段话：

> Connector/J did not support `utf8mb4` for servers `5.5.2` and newer.
> Connector/J now auto-detects servers configured with `character_set_server=utf8mb4` or treats the Java encoding `utf-8` passed
>  using characterEncoding=... as utf8mb4 in the SET NAMES= calls it makes when establishing the connection. (Bug #54175)

意思是，java驱动会自动检测服务端 `character_set_server` 的配置，如果`为utf8mb4`，驱动在建立连接的时候设置 `SET NAMES utf8mb4`。然而其他语言没有依赖于这样的特性。

## 5. 主从复制报错

这个问题没有遇到，只是看官方文档有提到，曾经也看到过类似的技术文章。

- MySQL版本不同：大概就是从库的版本比主库的版本低，导致有些字符集不支持；
- 主从字符集不同：人工修改了从库上的表或字段的字符集定义，都有可能引起异常。






