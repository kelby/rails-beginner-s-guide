## Schema Statements

重要的操作或问询如下(实例方法)：

```
# index(索引)
add_index
remove_index
rename_index
index_exists?
index_name_exists?

# column(属性)
add_column
rename_column
change_column
remove_column
remove_columns
column_exists?

# table(表)
create_table
drop_table
rename_table
change_table
table_exists?

# 外键
add_foreign_key
remove_foreign_key

# 引用(区别于外键，多态也可用)
add_reference & add_belongs_to
remove_belongs_to & remove_reference
```

其它操作或问询(实例方法)：

```
add_timestamps

assume_migrated_upto_version

change_column_default, change_column_null

initialize_schema_migrations_table

create_join_table
drop_join_table

columns
foreign_keys
native_database_types

remove_timestamps

table_alias_for
```

其它实例方法：

```
options_include_default?

add_index_sort_order
rename_column_indexes
rename_table_indexes

index_name_for_remove
quoted_columns_for_index
```
