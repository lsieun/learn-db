# SHA2() function

- **Syntax**: `SHA2(str, hash_length)`

Calculates the SHA-2 family of hash functions (`SHA-224`, `SHA-256`, `SHA-384`, and `SHA-512`). 

- **The first argument** is the **cleartext** string to be hashed. 
- **The second argument** indicates **the desired bit length** of the result, which must have a value of 224, 256, 384, 512, or 0 (which is equivalent to 256).  

If either argument is `NULL` or the hash length is not one of the permitted values, the return value is `NULL`. Otherwise, the function result is a hash value containing the desired number of bits.

## Examples

### One

```sql
SELECT SHA2('helloworld', 256);
```

Output:

```txt
+------------------------------------------------------------------+
| SHA2('helloworld', 256)                                          |
+------------------------------------------------------------------+
| 936a185caaa266bb9cbe981e9e05cb78cd732b0b3280eb944412bb6f8f8f07af |
+------------------------------------------------------------------+
```

### Two

```sql
SELECT SHA2("bob", 256) AS 'Hashed String',
    UNHEX(SHA2("bob", 256)) AS 'Binary String',
    LENGTH(UNHEX(SHA2("bob", 256))) AS 'Byte Count';
```

Output:

```txt
+------------------------------------------------------------------+------------+
| Hashed String                                                    | Byte Count |
+------------------------------------------------------------------+------------+
| 81b637d8fcd2c6da6359e6963113a1170de795e4b725b84d1e0b4cfd9ec58ce9 |         32 |
+------------------------------------------------------------------+------------+
```

