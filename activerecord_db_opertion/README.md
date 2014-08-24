# ActiveRecord 数据库操作

Web 应用使用到数据库，而管理数据库使用的是 SQL 语言。我们不需要专门去学习 SQL，只需要用 Ruby 语言，写 Ruby 代码就能实现数据库的相关操作(好吧，其实就是各种复杂的读写操作)。

## Transactions

事务

## CounterCache

加一、减一、重置、更新

```
increment_counter(counter_name, id)
decrement_counter(counter_name, id)
reset_counters(id, *counters)
update_counters(id, counters)
```


## Persistence

很重要

## Querying

很重要

##Relation

属于 ARel

## Scoping

```
default_scope(scope = nil)
unscoped

all
scope(name, body, &block)
```


## Calculations

取数据，并**统计**，有一点偏向数学：

```ruby
ids()
calculate(operation, column_name, options = {})

pluck(*column_names)

count(column_name = nil, options = {})
minimum(column_name, options = {})
maximum(column_name, options = {})
average(column_name, options = {})

sum(*args)
```

除了 `ids` 不需要，`count` 可选外，其余方法都至少要有一个属性。

> **Note:** 它们都是直接返回结果。

## QueryMethods

查表操作(数据库读操作)。大部分是Ruby层面，一般可多条件链式查询。

和上面的 CollectionProxy 有类似，区别在于这是对本表操作，而不是对关系表。和下面的 FinderMethods 有类似，区别在于这返回的是 relation，可以链式查询，与 SQL 关联大。

注意：参数带 block 的，代表已经进入SQL层面。

要善于使用这里的语句，数据放在数据库和数据放在内存有很大的区别！另外，放在内存和是否返回也有很大的区别！

joins 普通的查询条件，关联对象不需要放到内存。

```ruby
# 第一次调用，需要查询，花销一般；再次调用，也需要查询，花销一般。
# 目的：查询主表，关联表仅做为查询条件之一

posts = Post.joins(:comments)
  Post Load (0.1ms)  SELECT "posts".* FROM "posts" INNER JOIN "comments" ON "comments"."commentable_id" = "posts"."id" AND "comments"."commentable_type" = 'Post'
  posts.first.comments
  Comment Load (0.2ms)  SELECT "comments".* FROM "comments"  WHERE "comments"."commentable_id" = ? AND "comments"."commentable_type" = ?  [["commentable_id", 1], ["commentable_type", "Post"]]
```
includes 两个查询都要做，关联对象也需要放到内存。

```ruby
# 第一次调用，需要查询，花销大；再次调用，不需要查询，花销为零。
# 目的：查询主表和关联表

posts = Post.includes(:comments)
  Post Load (0.6ms)  SELECT "posts".* FROM "posts"
  Comment Load (0.2ms)  SELECT "comments".* FROM "comments"  WHERE "comments"."commentable_type" = 'Post' AND "comments"."commentable_id" IN (1, 3, 5, 6, 8, 9, 10, 11, 12)
  posts.first.comments

### 关键：后续是否需要对关联对象进行操作。
```

> **Note:** 返回的多是 Relation，与SQL层面较亲；有 find 字样的绝对不是它。

## FinderMethods & Batches
------

查表操作(数据库读操作)。大部分是SQL层面，一般不可多条件链式查询。

> **Note:** 返回的都是结果，不是 Relation。

## Relation(Arel)

对 Relation 的操作。哦，我们先理解概念。

- 为什么不完全用Ruby或SQL？

Ruby慢，人性化；SQL快，不易读写。

- 如何充分利用两者优点，特别是我们要做复杂查询的时候？(复杂查询 = 简单的查询 + 简单的查询 + ...)

"(Ruby查询 -> SQL查询，Ruby查询 -> SQL查询) -> 结果" 比 "(Ruby查询 + Ruby查询 -> SQL查询) -> 结果" 效率要低。所以尽量不要在Ruby - SQL两个层面之间转来转去，尽量把多个Ruby查询转化成相应的一个SQL查询。

- 如何实现？

运用中间状态，也就是Relation。每次查询并不是真正的查询(因为没走到SQL层面)，而是保存一个中间状态，当你所有的查询条件都写完了，才进入SQL的层面。理论上，这些简单的查询最后都能组合成SQL语句！

- 好处？

延迟加载。我们在Controller里有一个查询语句，结果赋值给一个实例变量，原本的意图是在View里显示的。某天需求更改了，我们不必再显示这个查询结果，但Controller里我们忘记删除这部分的代码(这都能忘？！)，结果每次都要做大量无用的查询工作。引入中间状态后，就能起到延迟加载的作用，不到万不得已，不做SQL查询(停留在Ruby层面)。

链式查询，而且效率高。上面已经提到了"(Ruby查询 -> SQL查询，Ruby查询 -> SQL查询) -> 结果" 比 "(Ruby查询 + Ruby查询 -> SQL查询) -> 结果" 效率要低。

- 如何区分？

坏消息：还没有好的办法区分它们。
好消息：大部分时间不必区分它们。

Relation 就类似没有名字的 scope 。当涉及跨表查询时，使用链式查询可以很大程度的提高效率。更多请查看接口 [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)

> **Note:** 这里部分是对多个对象的操作，对 Relation 的操作；不是查询操作。

## Persistence

数据库写操作，包括{update, destroy, create, save ...}

用原生 save 會有什麼問題呢？原生的 save 在資料儲存時，會經過一堆 validator 和 callbacks，即使你只是要簡單 update 一個欄位。

> **Note:** 这里大部分是对单个对象的操作。

