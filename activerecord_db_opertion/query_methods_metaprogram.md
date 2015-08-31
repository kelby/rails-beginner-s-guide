## Query Methods 读取、设置查询方法

**除【Query Methods】介绍的方法外，还有以下读取、设置查询方法。**它们都是用元编程生成的。

在 Relation 文件下，有代码：

```ruby
MULTI_VALUE_METHODS  = [:includes, :eager_load, :preload, :select, :group,
                        :order, :joins, :where, :having, :bind, :references,
                        :extending, :unscope]

SINGLE_VALUE_METHODS = [:limit, :offset, :lock, :readonly, :from, :reordering,
                        :reverse_order, :distinct, :create_with, :uniq]
```

在 Query Methods 文件下，使用它们。读取或设置指定的查询条件。

```ruby
MULTI_VALUE_METHODS.each do |name|
  def #{name}_values                # def select_values
    @values[:#{name}] || []         #   @values[:select] || []
  end                               # end
                                    #
  def #{name}_values=(values)       # def select_values=(values)
    @values[:#{name}] = values      #   @values[:select] = values
  end                               # end
end

(SINGLE_VALUE_METHODS - [:create_with]).each do |name|
  def #{name}_value                 # def readonly_value
    @values[:#{name}]               #   @values[:readonly]
  end                               # end
end

SINGLE_VALUE_METHODS.each do |name|
  def #{name}_value=(value)         # def readonly_value=(value)
    @values[:#{name}] = value       #   @values[:readonly] = value
  end                               # end
end
```

根据以上代码，生成方法：

```
includes_values
includes_values=

eager_load_values
eager_load_values=

preload_values
preload_values=

select_values
select_values=

group_values
group_values=

order_values
order_values=

joins_values
joins_values=

where_values
where_values=

having_values
having_values=

bind_values
bind_values=

references_values
references_values=

extending_values
extending_values=

unscope_values
unscope_values=
```

和 

```
limit_value
limit_value=

offset_value
offset_value=

lock_value
lock_value=

readonly_value
readonly_value=

from_value
from_value=

reordering_value
reordering_value=

reverse_order_value
reverse_order_value=

distinct_value
distinct_value=

create_with_value=

uniq_value
uniq_value=
```
