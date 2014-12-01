# ActiveRecord 工具

## No Touching

常用方法：

```
no_touching
```

使用举例：

```ruby
ActiveRecord::Base.no_touching do
  Project.first.touch # does nothing
  Message.first.touch # does nothing
end

Project.no_touching do
  Project.first.touch # does nothing
  Message.first.touch # works, but does not touch the associated project
end
```

除上述外，还有方法：

```
no_touching?
touch
```

## ~~Translation~~

## Readonly Attributes

提供方法：

```
attr_readonly

readonly_attributes
```

`attr_readonly` 和其它 attr_x 类似，只不过这里设置的是某属性为只读。注意，这里不是校验，所以保存出错的话，不会放到 record 对象的 errors 里。

## Attributes

提供方法

```
attribute

columns
columns_hash

reset_column_information
```

`columns` 可用于查看数据库里某张表有什么列。如：

```ruby
Book.columns.map{ |c| c.name }
```

下面主要讲解 `attribute` 方法

1 **覆盖原有类型的行为**

使用举例

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

2 **重新定义类型**

使用举例

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
