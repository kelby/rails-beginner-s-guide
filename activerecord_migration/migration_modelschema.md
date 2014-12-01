## Model Schema*

常用的如：

```
reset_column_information
```

整个迁移过程，环境只加载一次。但有时候，我们添加或删除了属性，我们希望马上能够填充数据。此时，就需要重新读取 model 的表信息，可以使用：

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
