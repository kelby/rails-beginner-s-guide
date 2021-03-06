## Attributes

提供方法：

```ruby
attribute

define_attribute
```

下面主要讲解 `attribute` 方法。

1) **覆盖原有类型的行为**

使用举例：

```ruby
# db/schema.rb
create_table :store_listings, force: true do |t|
  t.decimal :price_in_cents
end

# app/models/store_listing.rb
class StoreListing < ActiveRecord::Base
end

store_listing = StoreListing.new(price_in_cents: '10.1')

# before
store_listing.price_in_cents # => BigDecimal.new(10.1)

class StoreListing < ActiveRecord::Base
  attribute :price_in_cents, Type::Integer.new
end

# after
store_listing.price_in_cents # => 10
```

2) **重新定义类型**

使用举例：

```ruby
class MoneyType < ActiveRecord::Type::Integer
  # 新类型需实现 type_cast 方法
  def type_cast(value)
    if value.include?('$')
      price_in_dollars = value.gsub(/\$/, '').to_f
      price_in_dollars * 100
    else
      value.to_i
    end
  end
end

class StoreListing < ActiveRecord::Base
  attribute :price_in_cents, MoneyType.new
end

store_listing = StoreListing.new(price_in_cents: '$10.00')
store_listing.price_in_cents # => 1000
```

> Note: 覆盖或重新定义新的类型，也许并不是好的实践，使用之后会遇到新的问题。
