# SubString

第一个例子：

```sql
select substring("HELLOWORLD", 1, 5) as 'Result';
```

```txt
+--------+
| Result |
+--------+
| HELLO  |
+--------+
```

第二个例子：

```sql
select substring(replace(uuid(), '-', ''), 1, 16) as 'HALF_STR';
```

Output:

```txt
+------------------+
| HALF_STR         |
+------------------+
| 0fa7eb7c10c511e9 |
+------------------+
```

第三个例子：

```sql
select replace(uuid(), '-', '') as 'UUID';

set @random_str = substring(replace(uuid(), '-', ''), 1, 16);
SELECT @random_str;
```

Output:

```txt
+------------------+
| @random_str      |
+------------------+
| 7314034310c511e9 |
+------------------+
```
