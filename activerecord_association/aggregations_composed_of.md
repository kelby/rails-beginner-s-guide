我们在一张表里有几个类似字段，比如 customers 表有 address_street, address_city 字段，用于保存地址信息。好的做法，当然是把它们拆分出来，单独做成 address 表。但如果我们不想/能折分表成的话(改动太大，处理遗留问题等)，使用 `composed_of` 可以实现不用真正折分表，又能起到到分离的作用。

```ruby
class Customer < ActiveRecord::Base
  composed_of :address, mapping: [ %w(address_street street), %w(address_city city) ]
end
```

这里，把 address 当做关联对象。原 customer 的 address_street 和 address_city 分别映射成为 address 的 street 和 city 属性。

根据"约定优于配置"，关联对象 address 对应 class Address，我们实现它：

```ruby
class Address
  attr_reader :street, :city

  def initialize(street, city)
    @street, @city = street, city
  end
end
```

`composed_of(part_id, options = {})`

Customer 有 balance、address_street、address_city 字段。

```ruby
class Customer < ActiveRecord::Base
  # 把 balance 当做关联对象，amount 映射成为它的属性；对应着 class Money
  composed_of :balance, class_name: "Money", mapping: %w(balance amount)

  # 把 address 当做关联对象，street 和 city 映射成为它的属性；对应着 class Address
  composed_of :address, mapping: [ %w(address_street street), %w(address_city city) ]
end
```

可选参数 `:class_name, :mapping, :allow_nil, :constructor, :converter`，此外，你有下列读、写方法：

```ruby
# reader_method(name, class_name, mapping, allow_nil, constructor)
# writer_method(name, class_name, mapping, allow_nil, converter)

Customer#balance, Customer#balance=(money)
Customer#address, Customer#address=(address)
```

注意：我们没有 model Money 和 model Address，也没有它们对应的表，但要实现其对应的 class，类似：

```ruby
class Money
  include Comparable
  attr_reader :amount, :currency
  EXCHANGE_RATES = { "USD_TO_DKK" => 6 }

  def initialize(amount, currency = "USD")
    @amount, @currency = amount, currency
  end

  def exchange_to(other_currency)
    exchanged_amount = (amount * EXCHANGE_RATES["#{currency}_TO_#{other_currency}"]).floor
    Money.new(exchanged_amount, other_currency)
  end

  def ==(other_money)
    amount == other_money.amount && currency == other_money.currency
  end

  def <=>(other_money)
    if currency == other_money.currency
      amount <=> other_money.amount
    else
      amount <=> other_money.exchange_to(currency).amount
    end
  end
end

class Address
  attr_reader :street, :city
  def initialize(street, city)
    @street, @city = street, city
  end

  def close_to?(other_address)
    city == other_address.city
  end

  def ==(other_address)
    city == other_address.city && street == other_address.street
  end
end

# 关键点
class ClassName
  attr_reader :attr1, :attr2

  def initialize(attr1, attr2)
    @attr1, @attr2 = attr1, attr2
  end
end
```

然后就能这么操作：

```ruby
customer = Customer.new

customer.balance
=> #<Money:0x007f8dabd8c940 @amount=nil, @currency="USD">

customer.address
=> #<Address:0x007f8dabd87940 @street=nil, @city=nil>

# 实例化 customer 的 balance 关联对象
customer.balance = Money.new(20)     # sets the Money value object and the attribute
customer.balance                     # => Money value object
customer.balance.amount              # => 20
customer.balance.currency            # => "USD"
customer.balance.exchange_to("DKK")  # => Money.new(120, "DKK")
customer.balance > Money.new(10)     # => true
customer.balance == Money.new(20)    # => true
customer.balance < Money.new(5)      # => false

# 还有

customer.address_street = "Hyancintvej"
customer.address_city   = "Copenhagen"
# 实例化 customer 的 address 关联对象
customer.address        # => Address.new("Hyancintvej", "Copenhagen")
customer.address.street # => "Hyancintvej"
customer.address.city   # => "Copenhagen"

customer.address_street = "Vesterbrogade"
customer.address        # => Address.new("Hyancintvej", "Copenhagen")
customer.clear_aggregation_cache
customer.address        # => Address.new("Vesterbrogade", "Copenhagen")

customer.address = Address.new("May Street", "Chicago")
customer.address_street # => "May Street"
customer.address_city   # => "Chicago"
```

参考一下 has_one，让 composed_of 变得容易理解。使用 has_one 和 composed_of，两张表均属于一对一关系，只不过 composed_of 是我们想像出来的表，并不存在真正的数据库里。

> Note: composed_of 创建的是'值对象'，区别于一般的'实体对象'。值对象没有唯一身份标识，只有所有的值相等，两个值对象才相等；而实体对象，有唯一标识(如：id)，只要唯一标识相等，两个实体对象就相等了。

---


当我们表里有一些特殊字段，需要专门处理，但又不想分表的话(改动太大，处理遗留问题等)，这个方法提供了方便。即不用分表，又能起到分离的作用。

`composed_of(part_id, options = {})`

Customer 有 balance、address_street、address_city 字段。

```ruby
class Customer < ActiveRecord::Base
  # 把 balance 当做关联对象，amount 映射成为它的属性；对应着 class Money
  composed_of :balance, class_name: "Money", mapping: %w(balance amount)

  # 把 address 当做关联对象，street 和 city 映射成为它的属性；对应着 class Address
  composed_of :address, mapping: [ %w(address_street street), %w(address_city city) ]
end
```

可选参数 `:class_name, :mapping, :allow_nil, :constructor, :converter`，此外，你有下列读、写方法：

```ruby
# reader_method(name, class_name, mapping, allow_nil, constructor)
# writer_method(name, class_name, mapping, allow_nil, converter)

Customer#balance, Customer#balance=(money)
Customer#address, Customer#address=(address)
```

注意：我们没有 model Money 和 model Address，也没有它们对应的表，但要实现其对应的 class，类似：

```ruby
class Money
  include Comparable
  attr_reader :amount, :currency
  EXCHANGE_RATES = { "USD_TO_DKK" => 6 }

  def initialize(amount, currency = "USD")
    @amount, @currency = amount, currency
  end

  def exchange_to(other_currency)
    exchanged_amount = (amount * EXCHANGE_RATES["#{currency}_TO_#{other_currency}"]).floor
    Money.new(exchanged_amount, other_currency)
  end

  def ==(other_money)
    amount == other_money.amount && currency == other_money.currency
  end

  def <=>(other_money)
    if currency == other_money.currency
      amount <=> other_money.amount
    else
      amount <=> other_money.exchange_to(currency).amount
    end
  end
end

class Address
  attr_reader :street, :city
  def initialize(street, city)
    @street, @city = street, city
  end

  def close_to?(other_address)
    city == other_address.city
  end

  def ==(other_address)
    city == other_address.city && street == other_address.street
  end
end

# 关键点
class ClassName
  attr_reader :attr1, :attr2

  def initialize(attr1, attr2)
    @attr1, @attr2 = attr1, attr2
  end
end
```

然后就能这么操作：

```ruby
customer = Customer.new

customer.balance
=> #<Money:0x007f8dabd8c940 @amount=nil, @currency="USD">

customer.address
=> #<Address:0x007f8dabd87940 @street=nil, @city=nil>

# 实例化 customer 的 balance 关联对象
customer.balance = Money.new(20)     # sets the Money value object and the attribute
customer.balance                     # => Money value object
customer.balance.amount              # => 20
customer.balance.currency            # => "USD"
customer.balance.exchange_to("DKK")  # => Money.new(120, "DKK")
customer.balance > Money.new(10)     # => true
customer.balance == Money.new(20)    # => true
customer.balance < Money.new(5)      # => false

# 还有

customer.address_street = "Hyancintvej"
customer.address_city   = "Copenhagen"
# 实例化 customer 的 address 关联对象
customer.address        # => Address.new("Hyancintvej", "Copenhagen")
customer.address.street # => "Hyancintvej"
customer.address.city   # => "Copenhagen"

customer.address_street = "Vesterbrogade"
customer.address        # => Address.new("Hyancintvej", "Copenhagen")
customer.clear_aggregation_cache
customer.address        # => Address.new("Vesterbrogade", "Copenhagen")

customer.address = Address.new("May Street", "Chicago")
customer.address_street # => "May Street"
customer.address_city   # => "Chicago"
```

参考一下 has_one，让 composed_of 变得容易理解。使用 has_one 和 composed_of，两张表均属于一对一关系，只不过 composed_of 是我们想像出来的表，并不存在真正的数据库里。

> 题外话: composed_of 创建的是'值对象'，区别于一般的'实体对象'。值对象没有唯一身份标识，只有所有的值相等，两个值对象才相等；而实体对象，有唯一标识(如：id)，只要唯一标识相等，两个实体对象就相等了。
