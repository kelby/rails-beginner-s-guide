# ActiveRecord 关联

## Associations

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

has_and_belongs_to_many 意味着中间表没有 model. 因此，不能通过 id 进行查询，也没有 validate、callback，更加不能直接读取&设置其值。唯一的方便之处是：通过 `<<` 即可创建中间表数据。

当你需要 model、id、validate、callback 或对中间表数据进行操作时，建议改为使用 has_many :throght，这时可以通过 model 管理中间表数据，不必也不能再 `<<`


| Association      | has_and_belongs_to_many | has_many :through |
|--                |--                       |----               |
| AKA | habtm      | through association|
| Structure        | Join Table | Join Model|
| Primary Key | no | yes|
| Rich Association | no | yes|
| Proxy Collection | yes| no|
| Distinct Selection | yes | ~~no~~ yes|
| Self-Referential | yes | yes|
| Eager Loading | yes| yes|
| Polymorphism |  no | yes|
| N-way Joins | no | yes |

There's a lot of good stuff packed into that table, so let's break it down and see what it's all about.



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


## Associations

表间关系

```ruby
has_many(name, scope = nil, options = {}, &extension)
has_one(name, scope = nil, options = {})
belongs_to(name, scope = nil, options = {})
has_and_belongs_to_many(name, scope = nil, options = {}, &extension)
```

## CollectionProxy

继承于 Relation。Association proxies in Active Record are middlemen between the object that holds the association, known as the @owner, and the actual associated object, known as the @target. The kind of association any proxy is about is available in @reflection. That's an instance of the class ActiveRecord::Reflection::AssociationReflection.

表1对象.表2对象.action，也就是说对关系表进行操作 （表1对象.表2对象是个集合）

```ruby
class Blog < ActiveRecord::Base
  has_many :posts
end

blog = Blog.first
posts = blog.posts
```

`@owner` 表示 blog，`@target` 表示 posts，`@reflection` 表示 blog.posts
注意：posts 是死的，一经赋值，每一次调用值都一样；blog.posts 是活的，每一次调用值都有可能不同。

> Note: 一定是关联表.

## Reflection

例如校验时就用到。

指的关系！

大明(父) --> 小明(子)

那么构成一条关系。


