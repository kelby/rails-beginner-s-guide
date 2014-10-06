# ActiveRecord 工具

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

## Integration

```
cache_key(*timestamp_names)
```

和 Persistence 提供的 `touch(*names)` 配合起来很棒。

也有 `to_param` 但只是同名而矣。


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

## Nested Attributes

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

## ReadonlyAttributes

提供方法：

```
attr_readonly

readonly_attributes
```
