## Array Inquirer

```ruby
variants = ActiveSupport::ArrayInquirer.new([:phone, :tablet])

variants.phone?    # => true
variants.tablet?   # => true
variants.desktop?  # => false
```

```ruby
variants = ActiveSupport::ArrayInquirer.new([:phone, :tablet])

variants.any?                      # => true
variants.any?(:phone, :tablet)     # => true
variants.any?('phone', 'desktop')  # => true
variants.any?(:desktop, :watch)    # => false
```
