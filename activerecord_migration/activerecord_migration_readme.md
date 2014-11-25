## Migration 文件下的方法

重要的如：

```
up
down

revert
reversible
```

其它

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

```
run 或 migrate
     |
     v
 exec_migration
     |
     v
 up 或 down
```
