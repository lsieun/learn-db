# FOREIGN KEY

```sql
ALTER TABLE 表名 ADD FOREIGN KEY (字段名) REFERENCES 表名(字段名) ;

# 添加外键的格式:
ALTER TABLE yourtablename
    ADD [CONSTRAINT 外键名] FOREIGN KEY [id] (index_col_name, ...)
    REFERENCEStbl_name (index_col_name, ...)
    [ON DELETE{CASCADE | SET NULL | NO ACTION | RESTRICT}]
    [ON UPDATE{CASCADE | SET NULL | NO ACTION | RESTRICT}]
```

`ON DELETE`, `ON UPDATE` 各选项属性说明：

- `RESTRICT` ：父表删除(更新)且外键对应子表记录存在时，则不允许删除(更新)。
- `CASCADE` ：父表删除(更新)时，外键对应子表记录同时删除(更新)。
- `SET NULL` ：父表删除(更新)时，外键对应子表记录同时置为NULL.(必须允许为NULL）
- `NO ACTION`：父表删除(更新)且外键对应子表记录存在时，则不允许删除(更新)。



