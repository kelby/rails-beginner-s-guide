两部分: Ordered Options 和 ~~Ordered Hash~~.

## Ordered Options

可以以方法的形式调用 Hash 的 key，读、写其 value.

普通 Hash

```ruby
h = {}
h[:boy]  = 'John'
h[:girl] = 'Mary'
h[:boy]  # => 'John'
h[:girl] # => 'Mary'
```

使用 Ordered Options 后：

```ruby
h = ActiveSupport::OrderedOptions.new
h.boy  = 'John'
h.girl = 'Mary'
h.boy  # => 'John'
h.girl # => 'Mary'

# 原来的调用方式仍然有效

h[:boy]  = 'John'
h[:girl] = 'Mary'
h[:boy]  # => 'John'
h[:girl] # => 'Mary'
```

## ~~Ordered Hash~~

默认 Ruby 的 Hash 已经按照加入的顺序进行排序。
