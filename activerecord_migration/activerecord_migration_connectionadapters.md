下面这 4 个模块，来源 ConnectionAdapters，它下面还有很多模块，在此忽略其它 ...

## Schema Statements

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
add_reference & add_belongs_to
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

## Table Definition

**简单划分 create_table**

使用 create_table 时，传递给 block 的参数 `t` 就是此类型：  

```ruby
create_table :table_name do |t|
  # t.class
  # => ActiveRecord::ConnectionAdapters::TableDefinition
end
```

重要的如：

```
column
```

而 `t` 所支持的数据类型通过 `column` 对外开放：

其它方法：

```
remove_column

columns
index
primary_key
timestamps
belongs_to & references
```

> Note: 这里有一些方法是通过元编程生成的，查文档找不到它们，可能通过 ActiveRecord::ConnectionAdapters::TableDefinition.public_instance_methods(false) 查看。

## Table

**简单划分 change_table**

使用 create_table 时，传递给 block 的参数 `t` 就是此类型：

```ruby
change_table :table_name do |t|
  # t.class
  # => ActiveRecord::ConnectionAdapters::Table
end
```

相关模块或方法，可以参考：TableDefinition 和 SchemaStatements#create_table

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

除上述外，还有：

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

> Note: 这里有一些方法是通过元编程生成的，查文档找不到它们，可能通过 ActiveRecord::ConnectionAdapters::Table.public_instance_methods(false) 查看。

## Database Statements

常用的如：

```
execute
```

有时候，我们会在迁移文件里直接运行 SQL，使用上述方法，可以达到目的。

举例：

```ruby
class MakeJoinUnique < ActiveRecord::Migration
  def up
    execute "ALTER TABLE `pages_linked_pages` ADD UNIQUE `page_id_linked_page_id` (`page_id`,`linked_page_id`)"
  end

  def down
    execute "ALTER TABLE `pages_linked_pages` DROP INDEX `page_id_linked_page_id`"
  end
end
```

其它方法，不在此一一列举。

