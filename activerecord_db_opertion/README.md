# ActiveRecord 数据库操作

Web 应用使用到数据库，而管理数据库使用的是 SQL 语言。我们不需要专门去学习 SQL，只需要用 Ruby 语言，写 Ruby 代码就能实现数据库的相关操作(好吧，其实就是各种复杂的读写操作)。

## Querying

```
delegate :find, :take, :take!, :first, :first!, :last, :last!, :exists?, :any?, :many?, to: :all
delegate :second, :second!, :third, :third!, :fourth, :fourth!, :fifth, :fifth!, :forty_two, :forty_two!, to: :all
delegate :first_or_create, :first_or_create!, :first_or_initialize, to: :all
delegate :find_or_create_by, :find_or_create_by!, :find_or_initialize_by, to: :all
delegate :find_by, :find_by!, to: :all
delegate :destroy, :destroy_all, :delete, :delete_all, :update, :update_all, to: :all
delegate :find_each, :find_in_batches, to: :all
delegate :select, :group, :order, :except, :reorder, :limit, :offset, :joins,
         :where, :rewhere, :preload, :eager_load, :includes, :from, :lock, :readonly,
         :having, :create_with, :uniq, :distinct, :references, :none, :unscope, to: :all
delegate :count, :average, :minimum, :maximum, :sum, :calculate, to: :all
delegate :pluck, :ids, to: :all
```

除上述 delegate 方法外，还有：

```
find_by_sql
count_by_sql
```

## NullRelation

结合 `ActiveRecord::Relation#none` 可以用来表示和处理结果为空的 Relation.

## QueryMethods

有独立章节进行讲解。

## FinderMethods & Batches

查表操作(数据库读操作)。大部分是SQL层面，一般不可多条件链式查询。

> Note: 返回的都是结果，不是 Relation。

## Relation(Arel)

对 Relation 的操作。哦，我们先理解概念。

- 为什么不完全用 Ruby 或 SQL？

Ruby 慢，人性化；SQL 快，不易读写。

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

> Note: 这里部分是对多个对象的操作，对 Relation 的操作；不是查询操作。

## Locking

锁，分为乐观锁(Optimistic)和悲观锁(Pessimistic)。会有专门章节介绍。
