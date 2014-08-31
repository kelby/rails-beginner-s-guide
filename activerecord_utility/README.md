# ActiveRecord 工具

## Validations

和 Active Model 里的校验实现原理类似，但有一点不同：这里的校验需要从数据库'读'数据。

```
validates_associated(*attr_names)
validates_presence_of(*attr_names)
validates_uniqueness_of(*attr_names)
```

`include ActiveModel::Validations` 然后:
实现相应的校验器，利用 `validates_with` 调用校验器，得到最终校验方法。

`validates_associated(*attr_names)`

Validates whether the associated object or objects are all valid.
Works with any kind of association.

`validates_presence_of(*attr_names)`

Validates that the specified attributes are not blank (as defined by Object#blank?), and, if the attribute is an association, that the associated object is not marked for destruction. Happens by default on save.

`validates_uniqueness_of(*attr_names)`

The check for an existing value should be run from a class that isn't abstract. This means working down from the current class (self), to the first non-abstract class. Since classes don't know
their subclasses, we have to build the hierarchy between self and the record's class.

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

