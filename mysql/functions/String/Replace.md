# replace

```sql
replace(object,search,replace)
```

把object中出现search的全部替换为replace

第一个例子：

```sql
select replace('www.lsieun.cn','w','Ww') as 'Result';
```

Output:

```txt
+------------------+
| Result           |
+------------------+
| WwWwWw.lsieun.cn |
+------------------+
```

第二个例子：

```sql
select replace(uuid(), '-', '') as 'UUID';
```

Output:

```txt
+----------------------------------+
| UUID                             |
+----------------------------------+
| 197853be10c411e9bd42e006e6ccdf9c |
+----------------------------------+
```

例：把表table中的name字段中的aa替换为bb 

```sql
update table set name=replace(name,'aa','bb');
```
