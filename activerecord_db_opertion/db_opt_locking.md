## Locking

锁，分为乐观锁(Optimistic)和悲观锁(Pessimistic).

### 悲观锁(Pessimistic)

#### 特性

强烈的独占和排他。

它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。

#### 实现

往往依靠数据库提供的锁机制（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。

一个典型的依赖数据库的悲观锁调用：

```sql
select * from account where name="Erica" for update;
```

这条 sql 语句锁定了 account 表中所有符合检索条件（ name="Erica" ）的记录。 本次事务提交之前（事务提交时会释放事务过程中的锁），外界无法修改这些记录。

#### Rails 的悲观锁

lock、lock! 和 with_lock.

其中，lock 和 with_lock 都是封装 lock! 而来。

lock 相当于 lock! 的别名，但调用者可以是 relation 对象。  
with_lock 和事务捆绑在一起，并且参数可以是代码块。

举例：

```ruby
# 使用 lock，注意生成的 SQL
Account.lock.find(1)
# SELECT `accounts`.* FROM `accounts` WHERE `accounts`.`id` = 1 LIMIT 1 FOR UPDATE

# lock 结合 transaction 一起使用
Account.transaction do
  # select * from accounts where name = 'shugo' limit 1 for update
  shugo = Account.where("name = 'shugo'").lock(true).first
  yuko = Account.where("name = 'yuko'").lock(true).first
  shugo.balance -= 100
  shugo.save!
  yuko.balance += 100
  yuko.save!
end

# 使用 lock!
Account.transaction do
  # select * from accounts where ...
  accounts = Account.where(...)
  account1 = accounts.detect { |account| ... }
  account2 = accounts.detect { |account| ... }

  # account1 和 account2 只能是单个对象
  # select * from accounts where id=? for update
  account1.lock!
  account2.lock!
  account1.balance -= 100
  account1.save!
  account2.balance += 100
  account2.save!
end

# 使用 with_lock!
account = Account.first

# account 加上了锁，代码块加上了事务
account.with_lock do
  account.balance -= 100
  account.save!
end
```

#### 使用注意

1) 锁表，一个地方执行写的时候，另一个地方不能同时执行写操作，这没问题。但问题是，你也不能执行读操作。

2) 一个地方读数据，并赋值给对象。另一个地方在这之后执行了写操作，这个对象(脏数据)会覆盖已经更新过的数据，而不是报错。

> Note: 它是数据库级别的锁。

### 乐观锁(Optimistic)

#### 相对于悲观锁

乐观锁(Optimistic Locking) 相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。

悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。而乐观锁机制在一定程度上解决了这个问题。

#### 实现

大多是基于数据版本(Version)记录机制实现。

何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个 "version" 字段来实现。读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据。

#### Rails 的乐观锁

默认标识字段是 lock_version，当包含此属性时自动生效。

可以用 locking_column 更换默认的标识字段，如：

```ruby
class Person < ActiveRecord::Base
  self.locking_column = :lock_person
end
```

举例：

通常用法：

1) 给表添加 `:lock_version, :integer, default: 0` 属性。

```
# 首先，添加 lock_version 属性
add_column :products, :lock_version, :integer, :default => 0, :null => false
```

2) 在表单里使用此属性。(此处略)

3) 如果更新的是脏数据，会报错 `StaleObjectError`，可根据这个做相应处理。

```ruby
p1 = Product.find(1)
p2 = Product.find(1)

p1.name = "Michael"
p1.save

p2.name = "should fail"
p2.save
# 如果别人想再次更改，(脏数据)不会覆盖已经更新过的数据，而是会报错。
# => Raises a ActiveRecord::StaleObjectError
```

> Note: 它是应用级别的锁。
