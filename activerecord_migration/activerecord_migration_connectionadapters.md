## SchemaStatements

重要的如下：

```
# index
add_index
remove_index
rename_index
index_exists?
index_name_exists?

# column
add_column
rename_column
change_column
remove_column
remove_columns
column_exists?

# table
create_table
drop_table
rename_table
change_table
table_exists?

add_reference
add_belongs_to
```

其它

```
add_foreign_key, add_timestamps, assume_migrated_upto_version
change_column_default, change_column_null, columns, create_join_table
drop_join_table
foreign_keys
initialize_schema_migrations_table
native_database_types
remove_belongs_to,  remove_foreign_key, remove_reference, remove_timestamps
table_alias_for
```

## Instance Protected methods

```
add_index_sort_order
index_name_for_remove
options_include_default?
quoted_columns_for_index
rename_column_indexes
rename_table_indexes
```

## TableDefinition

**简单划分 create_table**

Represents the schema of an SQL table in an abstract way. This class provides methods for manipulating the schema representation.

Inside migration files, the t object in create_table is actually of this type:

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

The table definitions The Columns are stored as a ColumnDefinition in the columns attribute.


```
belongs_to
column, columns
index
primary_key
references, remove_column
timestamps
```

## Table

**简单划分 change_table**

Represents an SQL table in an abstract way for updating a table. Also see TableDefinition and ActiveRecord::ConnectionAdapters::SchemaStatements#create_table

Available transformations are:

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
belongs_to
change, change_default, column, column_exists?
index, index_exists?
references, remove, remove_belongs_to, remove_index, remove_references, remove_timestamps, rename, rename_index
timestamps
```
