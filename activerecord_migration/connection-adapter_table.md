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

列举如下

> 
:column, :column_exists?, :index, :index_exists?, :rename_index, :timestamps, :change, :change_default, :remove, :remove_index, :remove_timestamps, :rename, :references, :belongs_to, :remove_references, :remove_belongs_to, :string, :text, :integer, :float, :decimal, :datetime, :timestamp, :time, :date, :binary, :boolean

