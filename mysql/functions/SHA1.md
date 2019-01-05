# SHA1() function

- **Syntax**: `SHA1(str)`
- **MySQL Version**: 5.6

MySQL `sha1()` function calculates an SHA-1 160-bit checksum for a string.

The function returns a value as a binary string of 40 hex digits. If the string supplied as the argument is `NULL`, the function returns `NULL`.

## Examples

### One

```sql
SELECT SHA1('helloworld') AS 'Hashed String';
```

Output:

```txt
+------------------------------------------+
| Hashed String                            |
+------------------------------------------+
| 6adfb183a4a2c94a2f92dab5ade762a47889a5a1 |
+------------------------------------------+
```

### Two

```sql
SELECT SHA1('helloworld') AS 'Hashed String', LENGTH(SHA1('helloworld')) AS 'Length';
```

Output:

```txt
+------------------------------------------+--------+
| Hashed String                            | Length |
+------------------------------------------+--------+
| 6adfb183a4a2c94a2f92dab5ade762a47889a5a1 |     40 |
+------------------------------------------+--------+
```

### Three

```sql
SELECT SHA1(NULL) AS 'Hashed String';
```

Output:

```txt
+---------------+
| Hashed String |
+---------------+
| NULL          |
+---------------+
```
