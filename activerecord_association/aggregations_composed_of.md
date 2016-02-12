## Aggregations - composed_of 的

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

之后即可对 Address 的实例对象进行操作。

#### 如何使用？

Customer 有 balance，address_street、address_city 字段。

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

除了读、写方法外，composed_of 还创建管理了 Reflection 关联两者：

```ruby
reflection = ActiveRecord::Reflection.create(:composed_of, part_id, nil, options, self)
Reflection.add_aggregate_reflection self, part_id, reflection
```

注意：我们没有 model Money 和 model Address，也没有它们对应的表，所以要实现其对应的 class，类似：

```ruby
class Money
  include Comparable
  attr_reader :amount, :currency
  EXCHANGE_RATES = { "USD_TO_DKK" => 6 }

  def initialize(amount, currency = "USD")
    @amount, @currency = amount, currency
  end

  def exchange_to(other_currency)
    exchanged_amount = (amount *
                        EXCHANGE_RATES["#{currency}_TO_#{other_currency}"]).floor
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

# 实例化 customer 的 balance 关联对象
customer.balance = Money.new(20)    # sets the Money value object and the attribute
customer.balance                    # => Money value object
customer.balance.amount             # => 20
customer.balance.currency           # => "USD"
customer.balance.exchange_to("DKK") # => Money.new(120, "DKK")
customer.balance > Money.new(10)    # => true
customer.balance == Money.new(20)   # => true
customer.balance < Money.new(5)     # => false

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

#### 可选参数详解

| 参数 | 解释 |
| -- | -- | -- |
| class_name | 和其它关联一样，可以指定类名 |
| mapping | 旧字段与新字段的映射关系 <br> 旧字段在前，新字段在后 |
| allow_nil | 被关联的对象，其属性全部为空时，这个被关联的对象是否为 nil 对象 <br> 默认选项为 false |
| constructor | 如何初始化值对象 |
| converter | 给值对象赋值时，如何处理 |

```ruby
# 默认 allow_nil: false
customer composed_of :address, allow_nil: false
# 则有
customer = Customer.new
customer.address # 为 Address 对象，非 nil

# 设置 allow_nil: true
customer composed_of :address, allow_nil: true
# 则有
customer = Customer.new
customer.address # 为 NilClass 对象，为 nil

# 不建议使用 constructor 和 converter，而是用"类"来代替
# 因为测试、维护都不方便，特别是有一定复杂度的时候
composed_of :ip_address,
            class_name: 'IPAddr',
            mapping: %w(ip to_i),
            constructor: Proc.new { |ip| IPAddr.new(ip, Socket::AF_INET) },
            converter: Proc.new { |ip| ip.is_a?(Integer) ? IPAddr.new(ip, \n
                                       Socket::AF_INET) : IPAddr.new(ip.to_s) }

constructor # 第一次调用 x.ip_address 如何初始化
converter   # 调用 x.ip_address= 时，如何处理值对象
```

默认 **关联对象只有在第一次调用，才会初始化** 注意这个特点，否则你会发再一些奇怪现象。例如：

```ruby
customer = Customer.new

# 第一次调用，初始化关联对象
customer.address
=> #<Address:0x007f8dabd87940 @street=nil, @city=nil>

# 赋值
customer.address_street = "Hyancintvej"
customer.address_city   = "Copenhagen"

# 奇怪现象
# customer 的 address 关联对象之前已经被实例化了，所以上面赋值"不起作用"。
customer.address        # => #<Address:0x007f8dabd87940 @street=nil, @city=nil>

customer.save
customer.reload

# 保存、重新加载，发现上面赋值"已经起作用"。
customer.address
=> #<Address:0x007fe99a433a60 @city="Copenhagen", @street="Hyancintvej">
```
