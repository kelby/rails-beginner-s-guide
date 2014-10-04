介绍的方法都在 **ActiveRecord::QueryMethods**


提供方法：

|方法|解释|
|--|--|
| bind | 不知道干嘛用|
| create_with | 没找到合适的使用场景|
| distinct & uniq | 通常要配合其它查询方法使用，返回是 Relation. 否则使用的是数组的 uniq 返回的不是 Relation |
| eager_load |后文解释|
| extending | 给一个 scope 增加方法，返回的仍然是 scope. <br> 如果传递的是 block, 则可以直接调用 block 里面的方法, 如果传递的是 module, 则可以调用 module 里面的方法。<br> 不推荐直接使用，这会大大提高复杂度，但扩展时可以使用。|
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


`where(opts = :chain, *rest)`

- 传递字符串。

不过这容易引起注入攻击。

- 传递数组。

数组第一个元素做为查询条件(占位符'?')，后面的元素按顺序辅助完成。
数组第一个元素做为查询条件(占位符':key')，后面的元素放到一个哈希里，辅助完成。
数组第一个元素做为查询条件(占位符'%')，后面的元素转义后，按顺序辅助完成。

传递多个参数时，默认把它们当做数组处理，尽管没有使用中括号 []

- 传递哈希。

包括用花括号 {} 或者祼哈希

- 配合 joins

如果要 joins 其它表，where 里的查询语句参数和之前一样，但被 join 的表名要包含在查询条件里。

```ruby
User.joins(:posts).where({ "posts.published" => true })
User.joins(:posts).where({ posts: { published: true } })
```

`select(*fields)`

默认 find 查询所有属性，也就是 select *.

如果只想查询部分属性，你可以自己指定：

- 传递 block，返回数组

- 传递属性名(列)，结果仅包含这些项

`group(*args)`

根据所传递的属性，对结果进行分组。和 select 对比：

```ruby
User.select([:id, :name])
=> [#<User id: 1, name: "Oscar">, #<User id: 2, name: "Oscar">, #<User id: 3, name: "Foo">

User.group(:name)
=> [#<User id: 3, name: "Foo", ...>, #<User id: 2, name: "Oscar", ...>]
```

group 通常要配合其它查询语句或条件使用，单独使用会有去重效果(根据属性，只包含最后一条数据)。

`having(opts, *rest)`

和 group 对比：having是筛选条件，group是分组。

```ruby
Rails 里 having 不能脱离 group 单独使用。

Order.having('SUM(price) > 30').group('user_id')
```

`order(*args)`

需要知道：属性和规则。如果不指定规则，则用默认的。

`limit(value)` 和 `offset(value)`

指定最多获取多少条数据。

```ruby
User.limit(10) # generated SQL has 'LIMIT 10'
User.limit(10).limit(20) # generated SQL has 'LIMIT 20'
```

指定从第几条开始获取数据。

类比：分页时的'第几页'类似 offset, 而'每页多少条数据'类似 limit

`readonly(value = true)`

注意是数据库返回的结果就是只读，和 `attr_readonly` 是两码事。

```ruby
users = User.readonly
users.first.save
=> ActiveRecord::ReadOnlyRecord: ActiveRecord::ReadOnlyRecord
```

`reorder(*args)`

重新排序。调用了其他人写的 query 方法，但对排序不满意的，可以重新排序。

`joins(*args)`

如果要根据关联表来查询，joins 后面的 where 查询语句里要包含被 join 表的表名。

```ruby
Category.joins(:posts)
# => SELECT categories.* FROM categories
  INNER JOIN posts ON posts.category_id = categories.id

Post.joins(:category, :comments)
# => SELECT posts.* FROM posts
  INNER JOIN categories ON posts.category_id = categories.id
  INNER JOIN comments ON comments.post_id = posts.id

product = StandardProduct.first
product.catalogs.joins(:catalogs_standard_products).where('catalogs_standard_products.is_default = ?', true)
# => SELECT DISTINCT `catalogs`.* FROM `catalogs` INNER JOIN `catalogs_standard_products` `catalogs_standard_products_catalogs` ON `catalogs_standard_products_catalogs`.`catalog_id` = `catalogs`.`id` INNER JOIN `catalogs_standard_products` ON `catalogs`.`id` = `catalogs_standard_products`.`catalog_id` WHERE `catalogs_standard_products`.`standard_product_id` = 100011 AND (catalogs_standard_products.is_default = 1)
```

`includes(*args)` 和 joins 类似，唯一的区别在于：它是预先加载，第一次执行会慢，后续不再执行。

```ruby
Post.includes(:comments).where("comments.visible" => true)
```

`distinct(value = true)`  aliased as: uniq

`explain()` 看看转化成 SQL 是什么样。

> Note: 注意区分 Rails 里的 group 和 SQL 里的 group_by

相关SQL [SQL Functions](http://www.w3schools.com/sql/sql_functions.asp)

参考官方文档 [Active Record Query Interface](http://edgeguides.rubyonrails.org/active_record_querying.html)

## 最后

none - 返回一个空的 Relation，对后续操作很有用。可以充分利用 Relation 链式调用、延迟加载等特性。  

not - "与或非"里面的"非"，例如"IS NOT NULL"查询。

references - 使用 includes 的時候會有參照 table 的問題，Rails4 新增了一個 references 去明確指出參照的 table，但如果在 where 的參數內是直接用 hash 的 conditions，即可不用指定 referen<br>

order - Rails4 終於把  default_scope 的 order 調整成正常的順序，default_scope 的 order 永遠會在最後而不像 Rails 3 優先權永遠是最高的。同時 order 可以使用 hash 帶入，也不需要再用字串了。<br>

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

> Note: 返回的多是 Relation，与SQL层面较亲；有 find 字样的绝对不是它。
