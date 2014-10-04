# ActiveRecord 工具

## Validations

和 Active Model 里的校验实现原理类似，但有一点不同：这里校验的属性要是关联对象或要从数据库'读'数据。

1) `validates_associated(*attr_names)`

校验是否存在关联(关系)。可以同时校验多个关联(关系)

```ruby
class Book < ActiveRecord::Base
  has_many :pages
  belongs_to :library

  validates_associated :pages, :library
end
```

2) `validates_presence_of(*attr_names)`

校验(数据库里)是否存在着关联对象。

3) `validates_uniqueness_of(*attr_names)`

校验属性的值是否唯一。默认是对所有 record 进行校验，可以用 scope 或 conditions 指定约束条件。

```ruby
class Person < ActiveRecord::Base
  validates_uniqueness_of :user_name
end

# 加 scope 约束条件
# 同一 account 的 person, user_name 不能相同
# 不同 account 的 person, user_name 可以相同
class Person < ActiveRecord::Base
  validates_uniqueness_of :user_name, scope: :account_id
end

# 加 conditions 约束条件
# status 为 archived 的 article，title 不能相同
# status 为其它值的 article，title 可以相同
class Article < ActiveRecord::Base
  validates_uniqueness_of :title, conditions: -> { where.not(status: 'archived') }
end
```
> Note: Persistence(持久化)里的 save、save! 方法，只管 create 或 update 数据，是没有校验功能的。所以，这里创建了同名方法，在做真正的"保存"之前用来做校验工作，这也是一种技巧。

## Callbacks

用 Active Model 提供的 `define_model_callbacks(*callbacks)` 来创建回调(仅仅是名字)，然后再 Active Record 这里实现回调(内容)。

```
define_model_callbacks :initialize, :find, :touch, :only => :after
define_model_callbacks :save, :create, :update, :destroy
```

## Enum

```ruby
definitions.each do |name, values|
  klass.singleton_class.send(:define_method, name.to_s.pluralize)

  define_method("#{name}=")
  define_method(name)

  pairs.each do |value, i| # pairs 约等于 values

    define_method("#{value}?")
    define_method("#{value}!")
```

## Integration

```
cache_key(*timestamp_names)
```

和 Persistence 提供的 `touch(*names)` 配合起来很棒。

也有 `to_param` 但只是同名而矣。

## NestedAttributes

方法 `accepts_nested_attributes_for(*attr_names)`

attr_names由：一个或多个属性(association_name) 和 一个或多个可选参数(option)组成。

只接受options：

```
:allow_destroy
:reject_if
:limit
:update_only
```

- 当你声明嵌套属性时，Rails会自动帮你定义属性的写方法。

```ruby
# 摘录部分代码
def #{association_name}_attributes=(attributes)
  assign_nested_attributes_for_#{type}_association(:#{association_name}, attributes)
end
```

`association_name` 就是你声明的属性，例如：

```ruby
class Book < ActiveRecord::Base
  has_one :author
  has_many :pages

  accepts_nested_attributes_for :author, :pages
end
```

生成 `author_attributes=(attributes)` 和 `pages_attributes=(attributes)`

- 对于关联对象，会自动设置 `:autosave`

```ruby
# 摘录部分代码
reflection.autosave = true                     # 自动保存
add_autosave_association_callbacks(reflection) # 回调在自动保存时仍然有效
```

对于嵌套的属性，默认你可以执行写操作，但不能删除它们。如果你真的要这么做，也可以通过 `:allow_destroy` 来设置。如：

```ruby
class Member < ActiveRecord::Base
  has_one :avatar
  accepts_nested_attributes_for :avatar, allow_destroy: true
end

# 然后

member.avatar_attributes = { id: '2', _destroy: '1' }
member.avatar.marked_for_destruction? # => true
member.save
member.reload.avatar # => nil
```

自动保存多个嵌套属性，有的可能不符合校验，为了处理这种情况。你可以设置 `:reject_if`:

```ruby
class Member < ActiveRecord::Base
  has_many :posts
  accepts_nested_attributes_for :posts, reject_if: proc { |attributes| attributes['title'].blank? }
end

params = { member: {
  name: 'joe', posts_attributes: [
    { title: 'Kari, the awesome Ruby documentation browser!' },
    { title: 'The egalitarian assumption of the modern citizen' },
    { title: '' } # this will be ignored because of the :reject_if proc
  ]
}}

member = Member.create(params[:member])
member.posts.length # => 2
member.posts.first.title # => 'Kari, the awesome Ruby documentation browser!'
member.posts.second.title # => 'The egalitarian assumption of the modern citizen'
```

## Store

`store(store_attribute, options = {})` 以 JSON(也可以理解为Hash) 的形式存储数据。

举例，我们数据库里有 `name` 字段，我们想这样存储：

`name = { last_name: "Kelby", first_name: "Lee" }`

```ruby
class User < ActiveRecord::Base
  store :name, accessors: [ :last_name, :first_name ], coder: JSON
end

u = User.new(last_name: 'Kelby', first_name: 'Lee')
u.last_name                          # Accessor stored attribute
u.name[:last_name] = 'Denmark' # Any attribute, even if not specified with an accessor

# There is no difference between strings and symbols for accessing custom attributes
u.settings[:last_name]  # => 'Denmark'
u.settings['last_name'] # => 'Denmark'
```

`store_accessor(store_attribute, *keys)` 给已有 Hash 追加 key

```ruby
# Add additional accessors to an existing store through store_accessor
class User < ActiveRecord::Base
  store_accessor :name, :nickname
end
```

`stored_attributes` 查询一个字段有哪些可用的 key

```ruby
User.stored_attributes[:name] # [:last_name, :first_name, :nickname]
```

## Transactions

`transaction(options = {}, &block)` 要么同时成功，往下走；要么同时失败，回滚。

事务是恢复和并发控制的基本单位。

事务应该具有4个属性：原子性、一致性、隔离性、持久性。这四个属性通常称为 ACID 特性。

- 原子性(atomicity). 一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。
- 一致性(consistency). 事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。
- 隔离性(isolation). 一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
- 持久性(durability). 持久性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。

使用举例：

```ruby
ActiveRecord::Base.transaction do
  david.withdrawal(100)
  mary.deposit(100)
end
```

Rails 提供的事务，可以做为类方法，也可以做为实例方法进行调用。  
并且，一个事务里面可以操作多个对象：

```ruby
# 类方法
Account.transaction do
  balance.save!
  account.save!
end

# 实例方法
balance.transaction do
  balance.save!
  account.save!
end
```

`after_commit(*args, &block)` 和 `after_rollback(*args, &block)` 完全一样

默认 commit 包括： created, updated, 和 destroyed.
不过，你可以用可选参数 `:on` 指定其中的一个或多个:

```ruby
after_commit :do_foo, on: :create
after_commit :do_bar, on: :update
after_commit :do_baz, on: :destroy

after_commit :do_foo_bar, on: [:create, :update]
after_commit :do_bar_baz, on: [:update, :destroy]
```

原理，和普通的回调类似。使用 ActiveSupport 提供的 `set_callback(name, *filter_list, &block)` 完成。

## NoTouching
