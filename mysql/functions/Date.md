# 时间

## `NOW()`

查看当前时间：

```sql
SELECT NOW() AS 'Now Time';
```

Output:

```txt
+---------------------+
| Now Time            |
+---------------------+
| 2019-01-17 18:40:04 |
+---------------------+
```

## `current_timestamp()`

使用`current_timestamp()`

```sql
SELECT current_timestamp() AS 'Now Time';
```

Output:

```txt
+---------------------+
| Now Time            |
+---------------------+
| 2019-01-17 18:42:53 |
+---------------------+
```

更精确的时间格式

```sql
SELECT current_timestamp(3) AS 'Now Time';
```

Output:

```txt
+-------------------------+
| Now Time                |
+-------------------------+
| 2019-01-17 18:44:21.719 |
+-------------------------+
```

## `unix_timestamp()`

获取时间戳

```sql
SELECT unix_timestamp(current_timestamp(3)) AS 'TIMESTAMP';
```

Output:

```txt
+----------------+
| TIMESTAMP      |
+----------------+
| 1547722005.488 |
+----------------+
```

去掉小数点

```sql
SELECT REPLACE(unix_timestamp(current_timestamp(3)),'.','') AS 'TIMESTAMP';
```

Output:

```txt
+---------------+
| TIMESTAMP     |
+---------------+
| 1547722183871 |
+---------------+
```
