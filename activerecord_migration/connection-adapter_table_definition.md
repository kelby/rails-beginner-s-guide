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

列举如下：

> 
:indexes, :indexes=, :name, :temporary, :options, :as, :columns, :primary_key, :[], :column, :remove_column, :string, :text, :integer, :float, :decimal, :datetime, :timestamp, :time, :date, :binary, :boolean, :index, :timestamps, :references, :belongs_to, :new_column_definition
