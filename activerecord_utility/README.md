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

`transaction(options = {}, &block)` Transactions are protective blocks where SQL statements are only permanent if they can all succeed as one atomic action. The classic example is a transfer between two accounts where you can only have a deposit if the withdrawal succeeded and vice versa. Transactions enforce the integrity of the database and guard the data against program errors or database break-downs. So basically you should use transaction blocks whenever you have a number of statements that must be executed together or not at all.

使用举例：

```ruby
ActiveRecord::Base.transaction do
  david.withdrawal(100)
  mary.deposit(100)
end
```

一个事务，里面操作多个对象：

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

This callback is called after a record has been created, updated, or destroyed.
You can specify that the callback should only be fired by a certain action with the :on option:

```ruby
after_commit :do_foo, on: :create
after_commit :do_bar, on: :update
after_commit :do_baz, on: :destroy

after_commit :do_foo_bar, on: [:create, :update]
after_commit :do_bar_baz, on: [:update, :destroy]
```

Note that transactional fixtures do not play well with this feature. Please use the test_after_commit gem to have these hooks fired in tests.

原理，和普通的回调类似。使用 ActiveSupport 提供的 `set_callback(name, *filter_list, &block)` 完成。

## NoTouching
