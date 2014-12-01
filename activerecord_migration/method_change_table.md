### change_table

可选参数：

**:bulk**

尽可能的将更改表操作转换成一条 SQL 语句，一次执行。

如下面所示，一条 SQL 语句，可完成多个操作：

```sql
ALTER TABLE `users` ADD COLUMN age INT(11), ADD COLUMN birthdate DATETIME ...
```

好处是，这可以加快迁移速度。
