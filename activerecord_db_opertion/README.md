# ActiveRecord 数据库操作

Web 应用使用到数据库，而管理数据库使用的是 SQL 语言。我们不需要专门去学习 SQL，只需要用 Ruby 语言，写 Ruby 代码就能实现数据库的相关操作(好吧，其实就是各种复杂的读写操作)。

## CounterCache

按要求加减指定计数器的值、统计数目的加一、统计数目的减一、重置计数器的值。

```
update_counters(id, counters)

# 下面这两个方法基于 update_counters
increment_counter(counter_name, id)
decrement_counter(counter_name, id)

reset_counters(id, *counters)
```

这几条命令直接转化成 sql 语句，所以性能上要比普通的"给对象 的计数品赋值，然后保存对象"要快，并且准确性得到了更高的保证。

之前没有统计数目，新增统计数目，或之前的统计数目存在错误，使用 reset_counters 你可以又快、又准确的得到统计数目。

## Persistence

很重要

```
becomes, becomes!

decrement, decrement!
increment, increment!

delete, destroy, destroy!, destroyed?

new_record?
persisted?

reload

save, save!

toggle, toggle!

touch

update, update! (update_attributes, update_attributes!)
update_attribute
update_column, update_columns
```

数据库写操作，包括{update, destroy, create, save ...}

用原生 save 會有什麼問題呢？原生的 save 在資料儲存時，會經過一堆 validator 和 callbacks，即使你只是要簡單 update 一個欄位。

> Note: 这里大部分是对单个对象的操作。

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

## NullRelation

结合 `ActiveRecord::Relation#none` 可以用来表示和处理结果为空的 Relation.

## QueryMethods

提供方法：

|方法|解释|
|--|--|
| bind | 不知道干嘛用|
| create_with | 没找到合适的使用场景|
| distinct & uniq | 通常要配合其它查询方法使用，返回是 Relation. 否则使用的是数组的 uniq 返回的不是 Relation |
| eager_load |后文解释|
| extending | 给一个 scope 增加方法，返回的仍然是 scope. <br> 如果传递的是 block, 则可以直接调用 block 里面的方法, 如果传递的是 module, 则可以调用 module 里面的方法。<br> 不推荐直接使用，这会大大提高复杂度，但扩展时可以用。|
| from | 从符合条件的 record 开始 |
| group | 后文解释 |
| having | having 是分组(group)后的筛选条件，分组后的数据组内再筛选; where 则是在分组前筛选 |
| includes | 后文解释 |
| joins | 后文解释 |
| limit | 限制结果数目 |
| lock | 锁定结果 |
| none | 返回一个空的 Relation |
| offset | 类似 from, 但它传递的是"第几条"，并且数据不够的话可循环；而后者传递的是查询条件，不符合条件返回空 |
| order | 按条件排序 |
| preload | 后文解释 |
| readonly | 查询结果只读，不可写 |
| references | 后文解释 |
| reorder | 重新按条件排序。在这之前有排序的话，忽略它们 |
| reverse_order | 反转之前的排序结果 |
| rewhere | 重新按条件查询。在这之前的排序条件和它没有冲突的情况下，保留它们；有冲突的情况下，忽略它们 |
| select | 后文解释 |
| unscope | 查询条件可以有多个。使用 unscope 可以忽略其中的一个或多个 |
| where | 后文解释 |

## FinderMethods & Batches

查表操作(数据库读操作)。大部分是SQL层面，一般不可多条件链式查询。

> Note: 返回的都是结果，不是 Relation。

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

> Note: 这里部分是对多个对象的操作，对 Relation 的操作；不是查询操作。

```ruby
JoinOperation = Struct.new(:relation, :join_class, :on)

MULTI_VALUE_METHODS  = [:includes, :eager_load, :preload, :select, :group,
                        :order, :joins, :where, :having, :bind, :references,
                        :extending, :unscope]

SINGLE_VALUE_METHODS = [:limit, :offset, :lock, :readonly, :from, :reordering,
                        :reverse_order, :distinct, :create_with, :uniq]
INVALID_METHODS_FOR_DELETE_ALL = [:limit, :distinct, :offset, :group, :having]

VALUE_METHODS = MULTI_VALUE_METHODS + SINGLE_VALUE_METHODS

include FinderMethods, Calculations, SpawnMethods, QueryMethods, Batches, Explain, Delegation
```

## Scoping

虽然只有 4 个方法，但很实用。

```ruby
default_scope(scope = nil) - 设置默认 scope
unscoped - 跳过之前设置的 scope

all - all 方法，默认已经 scope
scope(name, body, &block) - 命名一个 scope
```

`scope(name, body, &block)` 重点说说这个方法。

### 相当于类方法，可检索、查询对象

可执行一系列的查询语句，如：where(color: :red).select('shirts.*').includes(:washing_instructions)

```ruby
class Shirt < ActiveRecord::Base
  scope :red, -> { where(color: 'red') }
  scope :dry_clean_only, -> { joins(:washing_instructions).where('washing_instructions.dry_clean_only = ?', true) }
end
```

The above calls to scope define class methods Shirt.red and Shirt.dry_clean_only. Shirt.red, in effect, represents the query Shirt.where(color: 'red').

You should always pass a callable object to the scopes defined with scope. This ensures that the scope is re-evaluated each time it is called.

Note that this is simply 'syntactic sugar' for defining an actual class method:

```ruby
class Shirt < ActiveRecord::Base
  def self.red
    where(color: 'red')
  end
end
```

Unlike Shirt.find(...), however, the object returned by Shirt.red is not an Array; it resembles the association object constructed by a has_many declaration. For instance, you can invoke Shirt.red.first, Shirt.red.count, Shirt.red.where(size: 'small'). Also, just as with the association objects, named scopes act like an Array, implementing Enumerable; Shirt.red.each(&block), Shirt.red.first, and Shirt.red.inject(memo, &block) all behave as if Shirt.red really was an Array.

These named scopes are composable. For instance, Shirt.red.dry_clean_only will produce all shirts that are both red and dry clean only. Nested finds and calculations also work with these compositions: Shirt.red.dry_clean_only.count returns the number of garments for which these criteria obtain. Similarly with Shirt.red.dry_clean_only.average(:thread_count).

All scopes are available as class methods on the ActiveRecord::Base descendant upon which the scopes were defined. But they are also available to has_many associations. If,

```ruby
class Person < ActiveRecord::Base
  has_many :shirts
end
```

then elton.shirts.red.dry_clean_only will return all of Elton's red, dry clean only shirts.

### scope 后可直接跟 extensions

和 has_many 类似的：

```ruby
class Shirt < ActiveRecord::Base
  scope :red, -> { where(color: 'red') } do
    def dom_id
      'red_shirts'
    end
  end
end
```

### scope 后可直接跟 creating/building 等方法

用于创建 record

```ruby
class Article < ActiveRecord::Base
  scope :published, -> { where(published: true) }
end

Article.published.new.published    # => true
Article.published.create.published # => true
```

### scope 后可直接跟类方法

定义如下：

```ruby
class Article < ActiveRecord::Base
  scope :published, -> { where(published: true) }
  scope :featured, -> { where(featured: true) }

  def self.latest_article
    order('published_at desc').first
  end

  def self.titles
    pluck(:title)
  end
end
```

调用如下:

```ruby
Article.published.featured.latest_article
Article.featured.titles
```

> Note: 并不是所有的方法都可以做为 scope 的内容，更多内容 [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html#retrieving-objects-from-the-database)

scopes - 一律使用 proc object 取代原本model內的 scope 的參數，proc object 或 block 取代原本 model 內的 default_scope 的參數。因為 Model 被 cache 的關係，會導致很多 eager-load 的 scope 只有在第一次載入的時候讀取正確的值(ex:時間)，這是 Rails2 和 Rails3 令人詬病的問題，而 Rails4 規定凡是 eager-load 的 scope 一律要使用 proc object。  

## Locking

锁，分为乐观锁(Optimistic)和悲观锁(Pessimistic)。会有专门章节介绍。

## AttributeAssignment

```
assign_attributes & attributes=
```

不推荐直接使用，因为和直接赋值效果一样，但扩展时可以使用。

使用举例：

```ruby
# 直接赋值
cat = Cat.new(name: "Gorby", status: "yawning")
cat.attributes # =>  { "name" => "Gorby", "status" => "yawning", "created_at" => nil, "updated_at" => nil}

# 使用 Attribute Assignment
cat.assign_attributes(status: "sleeping")
cat.attributes # =>  { "name" => "Gorby", "status" => "sleeping", "created_at" => nil, "updated_at" => nil }
```
