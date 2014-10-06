# OrderedHash 和 OrderedOptions

## OrderedOptions

普通 Hash

```ruby
h = {}
h[:boy] = 'John'
h[:girl] = 'Mary'
h[:boy]  # => 'John'
h[:girl] # => 'Mary'
```

使用 OrderedOptions

```ruby
h = ActiveSupport::OrderedOptions.new
h.boy = 'John'
h.girl = 'Mary'
h.boy  # => 'John'
h.girl # => 'Mary'
```

**扩展 Hash**

可以把 Hash 的 key 当做方法，可以读写 value。`OrderedOptions` 扩展于 Hash

普通的 Hash 是这样:

```ruby
  h = {}
  h[:boy] = 'John'
  h[:girl] = 'Mary'
  h[:boy]  # => 'John'
  h[:girl] # => 'Mary'
```

使用 OrderedOptions, Hash 的 key 可以当做方法，读写 value:

```ruby
  h = ActiveSupport::OrderedOptions.new
  h.boy = 'John'
  h.girl = 'Mary'
  h.boy  # => 'John'
  h.girl # => 'Mary'
```

默认 Hash 的 `keys` 方法，是没有排序的，使用 `OrderedHash` 后会进行排序

```ruby
oh = ActiveSupport::OrderedHash.new
oh[:a] = 1
oh[:c] = 3
oh[:b] = 2
oh.keys # => [:a, :b, :c]
# Hash 默认的 keys 没有排序，这里显示的是 [:a, :c, :b]
```

实现：语法糖。

