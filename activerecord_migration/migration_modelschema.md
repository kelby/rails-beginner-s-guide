## Model Schema

通过它们可以查看和更改 model 与 table 的对应关系。

**常用类方法：**

```
column_names
column_defaults

inheritance_column
inheritance_column=

reset_column_information

table_name
table_name=

table_exists?
```

在数据库层面，我们可以给某一列设置默认值。在 model 层面，我们创建对象之后，如果没有设置对应列的值，那保存的时候存的就是默认值。
<br>
那么，怎么查看所有列及其在数据库层面的默认值呢。
<br>
通过它：`column_defaults`

查看所有列的名字：`column_names`

单表继承时，默认字段为 `type`, 可以更改它：`inheritance_column=`

`reset_column_information` 定义于 Model Schema，在迁移的时候经常用到。

整个迁移过程，环境只加载一次。但有时候，我们添加了属性，我们希望马上能够填充数据。此时，就需要重新读取 model 的表信息。

举例：

```ruby
class AddPeopleSalary < ActiveRecord::Migration
  def up
    # 添加属性
    add_column :people, :salary, :integer
    # 确保已经添加了此属性
    Person.reset_column_information
    # 马上填充数据
    Person.all.each do |p|
      p.update_attribute :salary, SalaryCalculator.compute(p)
    end
  end
end
```

**其它类方法：**

```
sequence_name
sequence_name=

quoted_table_name

content_columns
```

`quoted_table_name` 可用命令的形式获取 Model 对应的表名。

`sequence_name` 获取序列名。
