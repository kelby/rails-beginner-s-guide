# Migration ModelSchema*

常用的如：

```
reset_column_information
```

迁移命令是一条条执行的，有时候我们添加了一列属性，希望马上就能填充数据。此时，为了确保数据库已经添加了此属性，可以使用上述命令在到目的。

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

其它方法，不在此一一列举。
