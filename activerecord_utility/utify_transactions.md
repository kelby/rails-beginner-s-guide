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
