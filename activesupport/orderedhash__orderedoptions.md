两部分: Ordered Options 和 Ordered Hash.

## Ordered Options

以方法的形式调用 Hash 的 key，读、写其 value.

普通 Hash：

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

## Ordered Hash

默认，Ruby 的 Hash 已经按照加入时的顺序进行排序，但这一点并不能得到保证，因为可能有"猴子补丁"的作用。

引入此模块后可，至少可当做 namespace，解决这一问题。

```ruby
oh = ActiveSupport::OrderedHash.new
oh[:a] = 1
oh[:b] = 2

# 确保 key 的顺序和加入时一样
oh.keys
# => [:a, :b]
```

