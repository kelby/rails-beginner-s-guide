## Migration 文件下的内容

重要的如：

```
run 或 migrate
     |
     v
 exec_migration
     |
     v
 up 或 down
```

```
revert
reversible
```

`revert` 内部其它方法会调用，懒人复制、粘贴，不想改名字使用。

其它：

```
check_pending!
disable_ddl_transaction!
load_schema_if_pending!

reverting?

run
migrate

exec_migration

announce
connection
copy
next_migration_number
proper_table_name
say
say_with_time
suppress_messages
table_name_options
write
```
