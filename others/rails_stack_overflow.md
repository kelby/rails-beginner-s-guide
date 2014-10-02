# Rails Stack OverFlow

## 如何使用 Rails migration 给表改列名？

答：

```ruby
def change
  rename_column :table_name, :old_column, :new_column
end
  
# 或
  
def self.up
  rename_column :table_name, :old_column, :new_column
end

def self.down
  # rename back if you need or do something else or do nothing
end
```

## 简洁明了的对比一下 nil & empty & blank

答：

|  | nil? | true( if condition) | empty?(string, array or hash) | blank? | present?(!blank?) |
| -- | -- | -- | -- | -- | -- |
| nil | √ | X |   | √ | X |
| false | X | X |   | √ | X |
| true | X | √ |   | X | √ |
| "" | X | √ | √ | √ | X |
| " " | X | √ | X | √ | X |
| [] | X | √ | √ | √ | X |
| [nil] | X | √ | X | X | √ |
| {}| X | √ | √ | √ | X |
| {temp: nil} | X | √ | X | X | √ |
| 0 | X | √ |   | X | √ |
| 5 | X | √ |   | X | √ |

blank? 和 present? 是 Rails 特有的方法。

除了 nil 本身外，其余元素 `nil?` 始终为 true

除了 false 和 nil 外，其余元素 `false?` 始终为 true

`empty?` 默认只能作用于 string、array、hash 对象

`blank?`

`blank?` 和 present? 是一对反义词




