# ActiveModel Errors

常用以下方法

| 方法 | 解释 |
|--|--|
| [] | 有则 get，无则 set |
| []= | 追加错误信息到属性 |
| add | 追加错误信息到属性，但在这之前做了一点格式化 |
|keys| 返回所有错误的 key |
|values| 返回所有错误的 value |
|each| 可以循环获取每一个错误的 key 和 value |
|full_messages| 返回所有的错误。但已经用 full_message 对它们进行了格式化，方便阅读 |
|blank? & empty?| errors 这个对象是否为空，也就是说是否有错误 |

除以上外，还有方法：

```
added?

set, get
size, count
clear, delete
has_key? & include?
add_on_blank, add_on_empty
to_a, to_hash, to_xml, as_json

full_message
initialize_dup
generate_message
```

以上常用与非常用方法，由作者根据经验划分，仅供参考。

视图里常用写法：

```ruby
<% if @record.errors[:title].present? %>
  <%= @record.errors[:title].join('') %>
<% end %>
```
