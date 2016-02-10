## Array Inquirer

元素后面加 `?`，可直接查询是否在数组内：

```ruby
variants = ActiveSupport::ArrayInquirer.new([:phone, :tablet])

variants.phone?    # => true
variants.tablet?   # => true
variants.desktop?  # => false
```

使用 `any?` 查询元素是否在数组内：

```ruby
variants = ActiveSupport::ArrayInquirer.new([:phone, :tablet])

variants.any?                      # => true
variants.any?(:phone, :tablet)     # => true
variants.any?('phone', 'desktop')  # => true
variants.any?(:desktop, :watch)    # => false
```
