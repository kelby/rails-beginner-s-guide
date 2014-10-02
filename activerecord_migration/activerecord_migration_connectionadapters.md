## SchemaStatements

重要的操作或问询如下：

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
add_reference
add_belongs_to
```

其它操作或问询：

```
add_foreign_key, add_timestamps

assume_migrated_upto_version

change_column_default, change_column_null

initialize_schema_migrations_table

create_join_table
drop_join_table

columns
foreign_keys
native_database_types

remove_belongs_to, remove_foreign_key, remove_reference, remove_timestamps

table_alias_for
```

## Instance Protected methods

```
options_include_default?

add_index_sort_order
rename_column_indexes
rename_table_indexes

index_name_for_remove
quoted_columns_for_index
```

## TableDefinition

**简单划分 create_table**

使用 create_table 时，传递给 block 的参数 `t` 就是此类型：  

```ruby
class SomeMigration < ActiveRecord::Migration
  def up
    create_table :foo do |t|
      puts t.class  # => "ActiveRecord::ConnectionAdapters::TableDefinition"
    end
  end

  def down
    ...
  end
end
```

而 `t` 所支持的数据类型通过 `column` 对外开放：

```
column

remove_column

columns
index
primary_key
timestamps
belongs_to & references
```

## Table

**简单划分 change_table**

相关模块或方法：TableDefinition 和 SchemaStatements#create_table

支持的数据类型或操作有:

```ruby
change_table :table do |t|
  t.column
  t.index
  t.rename_index
  t.timestamps
  t.change
  t.change_default
  t.rename
  t.references
  t.belongs_to
  t.string
  t.text
  t.integer
  t.float
  t.decimal
  t.datetime
  t.timestamp
  t.time
  t.date
  t.binary
  t.boolean
  t.remove
  t.remove_references
  t.remove_belongs_to
  t.remove_index
  t.remove_timestamps
end
```

```
column_exists?
index_exists?

change, change_default

remove, remove_index, remove_timestamps
remove_belongs_to & remove_references

rename, rename_index

column
index
timestamps
belongs_to & references
```
