##其它


### Integration

`to_param()`

默认，Rails 生成URL时用的是 `primary key`，也就是数据库里的 `id` 属性。例如：

```ruby
user = User.find_by(name: 'Phusion')
user_path(user)  # => "/users/1"
```

这对于SEO和人类识别都不是很友好。我们可以重写 `to_param` 方法，设置更友好的内容：

```ruby
class User < ActiveRecord::Base
  def to_param  # overridden
    name
  end
end

user = User.find_by(name: 'Phusion')
user_path(user)  # => "/users/Phusion"
```

`cache_key(*timestamp_names)`

返回一个能够标识对象的字符串：

```ruby
Product.new.cache_key     # => "products/new"
Product.find(5).cache_key # => "products/5" (updated_at not available)
Person.find(5).cache_key  # => "people/5-20071224150000" (updated_at available)
```

当我们需要缓存的地方很多时，默认生成字符串的规则可能满足不了我们的需求。我们可以传递参数给它，用新的规则生成字符串：

```ruby
Person.find(5).cache_key(:updated_at, :last_reviewed_at)
```

### Enum

假设，我们有 `status` 字段，那么使用 `enum` 方法会引入什么魔法：

```ruby
class Post < ActiveRecord::Base
  enum status: [ :active, :archived ]
end
```

```ruby
# 1 同名类方法(复数形式)
Post.statuses
=> {"active"=>0, "archived"=>1}

# 2 值(scope)
Post.active
  Post Load (1.9ms)  SELECT "posts".* FROM "posts"  WHERE "posts"."status" = 0

post = Post.first

# 3 值?
post.active?
=> false

# 4 值!
post.active!
   (0.1ms)  begin transaction
  SQL (0.4ms)  UPDATE "posts" SET "status" = ?, "updated_at" = ? WHERE "posts"."id" = 2  [["status", 0], ["updated_at", "2014-04-20 09:06:53.722202"]]
   (8.7ms)  commit transaction
=> true

# 5 同名实例方法(get)
post.status
=> "active"

# 6 同名实例方法=(set)
post.status = "archived"
=> "archived"

# 7 和同名实例方法(get)作用一样
post.status_before_type_cast
=> "archived"
```

它们对应的源代码：

```ruby
def enum(definitions)
  klass = self
  definitions.each do |name, values|
    # statuses = { }
    enum_values = ActiveSupport::HashWithIndifferentAccess.new
    name        = name.to_sym

    # def self.statuses statuses end
    detect_enum_conflict!(name, name.to_s.pluralize, true)
    # 1 同名类方法(复数形式)
    klass.singleton_class.send(:define_method, name.to_s.pluralize) { enum_values }

    _enum_methods_module.module_eval do
      # def status=(value) self[:status] = statuses[value] end
      klass.send(:detect_enum_conflict!, name, "#{name}=")
      # 6 同名实例方法=(set)
      define_method("#{name}=") { |value|
        if enum_values.has_key?(value) || value.blank?
          self[name] = enum_values[value]
        elsif enum_values.has_value?(value)
          # Assigning a value directly is not a end-user feature, hence it's not documented.
          # This is used internally to make building objects from the generated scopes work
          # as expected, i.e. +Conversation.archived.build.archived?+ should be true.
          self[name] = value
        else
          raise ArgumentError, "'#{value}' is not a valid #{name}"
        end
      }

      # def status() statuses.key self[:status] end
      klass.send(:detect_enum_conflict!, name, name)
      # 5 同名实例方法(get)
      define_method(name) { enum_values.key self[name] }

      # def status_before_type_cast() statuses.key self[:status] end
      klass.send(:detect_enum_conflict!, name, "#{name}_before_type_cast")
      # 7 和同名实例方法(get)作用一样
      define_method("#{name}_before_type_cast") { enum_values.key self[name] }

      pairs = values.respond_to?(:each_pair) ? values.each_pair : values.each_with_index
      pairs.each do |value, i|
        enum_values[value] = i

        # def active?() status == 0 end
        klass.send(:detect_enum_conflict!, name, "#{value}?")
        # 3 值?
        define_method("#{value}?") { self[name] == i }

        # def active!() update! status: :active end
        klass.send(:detect_enum_conflict!, name, "#{value}!")
        # 4 值！
        define_method("#{value}!") { update! name => value }

        # scope :active, -> { where status: 0 }
        klass.send(:detect_enum_conflict!, name, value, true)
        # 2 值(scope)
        klass.scope value, -> { klass.where name => i }
      end
    end
    defined_enums[name.to_s] = enum_values
  end
end
```

### CounterCache

`update_counters(id, counters)`

计数器实现。在它基础上通常有加一、减一操作，但也可以单独使用。
考虑到并发，这里并不只是Rails层面的get，然后set。而是在SQL层面"真正执行时"才增量加减(但没有用锁机制)。下面的加一、减一方法都是这样。

```ruby
# 注意：跟的是字段名
Post.update_counters 3, comments_count: +1
  SQL (8.9ms)  UPDATE "posts" SET "comments_count" = COALESCE("comments_count", 0) + 1 WHERE "posts"."id" = 3
=> 1

# 新增计数器: 原来没有使用计数器的，现在我们希望添加一个计数器。因为数据已经存在了，我们需要设置正确的数目。
# Post.update_counters 3, comments_count: post.comments.count
```

`increment_counter(counter_name, id)`

给 `counter_name` 字段进行加一。

`decrement_counter(counter_name, id)`

给 `counter_name` 字段进行减一。

`reset_counters(id, *counters)`

上文提到的"新增计数器"，也可用此方法实现。

```ruby
# 注意：跟的关系表名(可以是多个)，但不是字段名
Post.reset_counters(3, :comments)
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  WHERE "posts"."id" = ? LIMIT 1  [["id", 3]]
   (0.2ms)  SELECT COUNT(*) FROM "comments"  WHERE "comments"."post_id" = ?  [["post_id", 3]]
   (9.4ms)  UPDATE "posts" SET "comments_count" = 2 WHERE "posts"."id" = 3
=> true
```

基于SQL层面，重置(理解为校正，而不是归零)一个或多个计数器的值。计数器有时候会不准，特别是我们用来计数关联对象的个数，而自己又手动删除它们。

> Note: counter_cache 的 attribute 默认是 read_only

### Callbacks

基于 ActionModel 提供的 `define_model_callbacks` 方法，共生成十几个过滤器方法。

```ruby
include ActiveModel::Validations::Callbacks

define_model_callbacks :initialize, :find, :touch, :only => :after
define_model_callbacks :save, :create, :update, :destroy
```

和 AbstractController::Callbacks::ClassMethods 用元编程生成过滤器的方法名，是两种手法(尽管最终都是基于ActiveSupport::Callbacks)。

### Aggregations

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

### Locking

`Optimistic`

默认，使用 `lock_version` 做为乐观锁的版本。新增乐观锁：

> add_column :posts, :lock_version, :integer, default: 0, null: false

举例：当你保存时会自动更新 lock_version 的值。

```ruby
p1.save!
   (0.1ms)  begin transaction
   (0.4ms)  UPDATE "posts" SET "title" = 'xxoo ', "updated_at" = '2014-04-20 14:02:58.425646', "lock_version" = 1 WHERE ("posts"."id" = 1 AND "posts"."lock_version" = 0)
   (8.1ms)  commit transaction
=> true

# 如果别人想再次更改，(脏数据)不会覆盖已经更新过的数据，而是会报错。
ActiveRecord::StaleObjectError: Attempted to update a stale object: Post
```

你也可以更改名字：

```ruby
class Post < ActiveRecord::Base
  self.locking_column = :lock_person
end
```

通常用法：

1. Add `:lock_version, :integer, default: 0` column to model
2. Use that column in forms for your model
3. Catch and resolve `StaleObjectError` on your model updates

缺点：有可能脏读。

它是应用级别的锁。

`Pessimistic`

例如 `Blog.find(1, lock: true)` 或 例如 `Blog.lock.find(1)`

```ruby
Blog.transaction do
  b = Blog.find(2, :lock=> true)
  b.title = 'How to Replace CLI-8 Chip2'
  sleep(20)
  b.save!
end
```

缺点：锁表，一个地方执行写的时候，另一个地方不能同时执行写操作，这没问题；但问题是，你也不能执行读操作。
缺点：一个地方读数据，并赋值给对象；另一个地方在这之后执行了写操作；这个对象(脏数据)会覆盖已经更新过的数据，而不会报错。

它是数据库级别的锁。

> Note: 一定要用锁的话，我推荐尽可能用乐观锁，但请从实际情况出发。

### AttributeMethods

太多了，真正涉及的时候再讲吧。

## 场外

array 是引用对象
[1, 2, 3] 是值对象

Relation 就相当于引用对象。

引进 Relation 概念，主要就是为了充分利用 SQL 层面的东西(读写)，毕竟执行 SQL 比执行 Ruby 代码速度(效率)更高。但语法用的还是 Ruby 这一套，毕竟它更友好(符合人类阅读习惯)。

----------

ActiveRecord 里设置表名、列名除了就对不规则外，还有一个功能是应对遗留数据库。如：
table_name_prefix
primary_key_prefix_type
pluralize_table_names
primary_key
等
