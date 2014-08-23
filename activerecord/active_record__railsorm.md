Active Record – Rails的ORM
=========

Web 应用使用到数据库，而管理数据库使用的是 SQL 语言。我们不需要专门去学习 SQL，只需要用 Ruby 语言，写 Ruby 代码就能实现数据库的相关操作(好吧，其实就是各种复杂的读写操作)。

##类映射表，属性映射列

使用 Rails 框架时，这个不由我们考些。

##Associations



```ruby
has_many(name, scope = nil, options = {}, &extension)
    def valid_options
      super + [:primary_key, :dependent, :as, :through, :source, :source_type, :inverse_of, :counter_cache]
    end
has_one(name, scope = nil, options = {})
    def valid_options
      valid = super + [:order, :as]
      valid += [:through, :source, :source_type] if options[:through]
      valid
    end
belongs_to(name, scope = nil, options = {})
    def valid_options
      super + [:foreign_type, :polymorphic, :touch, :counter_cache]
    end
has_and_belongs_to_many(name, scope = nil, options = {}, &extension)

#---------------------

ActiveRecord::Associations::Builder::Association.build(model, name, scope, options, &block)
  if model.dangerous_attribute_method?(name)
    raise ArgumentError, "You tried to define an association named #{name} on the model #{model.name}, but " \
                         "this will conflict with a method #{name} already defined by Active Record. " \
                         "Please choose a different association name."
  end

  builder = create_builder model, name, scope, options, &block
  reflection = builder.build(model)
  define_accessors model, reflection
  define_callbacks model, reflection
  builder.define_extensions model
  reflection
end

#---------------------

ActiveRecord::Reflection.add_reflection(ar, name, reflection)
  ar.reflections = ar.reflections.merge(name => reflection)
end
```

Why do we need associations between models? Because they **make common operations simpler and easier** in your code.

英文环境里：

a has_many :b
和
a belongs_to :b

都有：
a(前者) 为 associated
b(后者) 为 association

:class_name指定b的类名
:foreign_key指定a_id
:primary_key指定b的id
:dependent对a进行操作，同时也对b进行操作
:counter_cache记录b的个数的字段
:as多态时，a相当于d
:through指定a通过c来关联b
:source与through配合，a直接对应的是b还是c
:source_type与through配合
:inverse_of从a查找b，与从b查找a是不同步的；有时候会引起错误，用此参数让它们一致。(为什么不始终使用？)对:through、 :polymorphic、 :as associations不起作用并且 对于 belongs_to 关联, has_many 反转关联会被自动忽略。
（:conditions、:foreign_key也会被忽略）

b belongs_to :a
:touch 修改b时，a也会被更新

**Scopes**

```
where
includes
readonly
select
```

**Scopes**

```
where
extending
group
includes
limit
offset
order
readonly
select
uniq
```

下文提到的方法里，哪些参数是对 associated 起作用，哪些是对 association 起作用？

用关系弄数据库作存储时，两个表之间是有关联的。放到 Web 应用里，就是两个 model类 之间是有关联的。通过Associations子模块，我们可以快速的实现，一对一、一对多和多对多等关系。

 - belongs_to

 - has_one 用于一对一关系
 - has_one :through 另一种方式实现一对一关系

 - has_many 用于一对多关系

 - has_and_belongs_to_many 用于多对多关系
 - has_many :through 另一种方式实现多对多关系

```ruby
class Customer < ActiveRecord::Base
end

class Order < ActiveRecord::Base
end

# To add an order
@order = Order.create(order_date: Time.now, customer_id: @customer.id)

# To delete orders
@order = Order.where(customer_id: @customer.id)
@order.each do |order|
  order.destroy
end
@customer.destroy
```

关联：

```
class Customer < ActiveRecord::Base
  has_many :orders, dependent: :destroy
end

class Order < ActiveRecord::Base
  belongs_to :customer
end

# To add an order
@order = @customer.orders.create(order_date: Time.now)

# To delete orders
@customer.destroy
```

`create_table` 里的 *:id => false*

除了上述几种情况外，一般还会遇到：

**多态**和**自关联**通过给上述几个方法传递不同的参数，可以达到效果。

其它一些有意思的可选参数：*:include*, *counter_cache*, *:touch*

[Active Record Associations](http://edgeguides.rubyonrails.org/association_basics.html)

##Validations

和 Active Model 里的校验实现原理类似，但有一点不同：这里的校验需要从数据库'读'数据。

```
validates_associated(*attr_names)
validates_presence_of(*attr_names)
validates_uniqueness_of(*attr_names)
```

##Callbacks

用 Active Model 提供的 `define_model_callbacks(*callbacks)` 来创建回调(仅仅是名字)，然后再 Active Record 这里实现回调(内容)。

```
define_model_callbacks :initialize, :find, :touch, :only => :after
define_model_callbacks :save, :create, :update, :destroy
```

##Transactions

事务

##Migrations

不必专门学习 SQL，即可修改数据库表结构。

本身实现了：

```
up()
down()
migrate()
```

它下面的 **CommandRecorder** 实现了：

```
add_column
add_index
add_reference
add_timestamps

create_table
create_join_table

drop_table (must supply a block)
drop_join_table (must supply a block)

remove_timestamps
remove_reference
remove_column
remove_columns
remove_index

rename_column
rename_index
rename_table

change_column_default
change_column
change_column_null
change_table

transaction
execute
execute_block
enable_extension
```

[Active Record Migrations](http://edgeguides.rubyonrails.org/migrations.html)

##CounterCache

加一、减一、重置、更新

```
increment_counter(counter_name, id)
decrement_counter(counter_name, id)
reset_counters(id, *counters)
update_counters(id, counters)
```

##Enum

```ruby
definitions.each do |name, values|
  klass.singleton_class.send(:define_method, name.to_s.pluralize)

  define_method("#{name}=")
  define_method(name)

  pairs.each do |value, i| # pairs 约等于 values

    define_method("#{value}?")
    define_method("#{value}!")
```

##Integration

```
cache_key(*timestamp_names)
```

和 Persistence 提供的 `touch(*names)` 配合起来很棒。

也有 `to_param` 但只是同名而矣。

##NestedAttributes

方法 `accepts_nested_attributes_for(*attr_names)`

attr_names由：一个或多个属性(association_name) 和 一个或多个可选参数(option)组成。

只接受options：

```
:allow_destroy
:reject_if
:limit
:update_only
```

##Persistence

很重要

##Querying

很重要

##Relation

属于 ARel

##Scoping

```
default_scope(scope = nil)
unscoped

all
scope(name, body, &block)
```

##外记：

###Store

```
store(store_attribute, options = {})
store_accessor(store_attribute, *keys)
```


