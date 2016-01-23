## Errors

有校验就会有失败，对属性校验失败时的报错，是它的实例。

```ruby
record.errors.class
=> ActiveModel::Errors
```

自己写校验方法或者校验器的时候，请务必设置 errors 的值，并且不通过进返回 false.

常用以下方法：

| 方法 | 解释 |
|--|--|
| [] | 有则 get，无则 set |
| add | 追加错误信息到属性，但在这之前做了一点格式化 |
|keys| 返回所有错误的 key |
|values| 返回所有错误的 value |
|each| 可以循环获取每一个错误的 key 和 value |
|full_messages| 返回所有的错误。但已经用 full_message 对它们进行了格式化，方便阅读 |
|blank? & empty?| errors 这个对象是否为空，也就是说是否有错误 |
| details | 查看 errors 详情 |

除以上外，还有方法：

```
added?

size
count

clear
delete

has_key? & include?

to_a
to_hash
to_xml
as_json

full_message
full_messages_for

initialize_dup

generate_message
```

视图里常用写法举例：

```ruby
<%= @record.errors.full_messages.join(";") %>

<%= @record.errors.full_messages.first %>
```

```ruby
<% if @record.errors[:title].present? %>
  <%= @record.errors[:title].join('') %>
<% end %>
```

自己定义的校验举例：

```ruby
# 校验开始工作日期不能早于今天
before_save :validates_of_beginning_work_date

def validates_of_beginning_work_date
  if self.beginning_work_date_changed? && (self.beginning_work_date < Time.now.beginning_of_day)
    # errors 不为空，做提醒。回调返回 false 就不能保存。
    self.errors.add :beginning_work_date, '不能小于当前时间'
    return false
  end
end

# 注意在 local 里对 beginning_work_date 进行翻译，以便更友好的显示报错信息。
```

> Note: 因为 include Enumerable，所以可以看出很多与其同名的方法。
