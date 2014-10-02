介绍的方法都在 **ActiveRecord::QueryMethods**

```ruby
bind
create_with
distinct - Also aliased as: uniq
eager_load
extending
from
group
having
includes
joins
limit
lock
none - 没找到
offset
order
preload
readonly
references - 没找到
reorder
reverse_order
rewhere - 没找到
select - 比较特殊，如果传递的参数里有 block，返回的是 Array，不是 Relation
uniq
unscope - 没找到
where
```

```ruby
where - provides conditions on the relation, what gets returned.
select - choose what attributes of the models you wish to have returned from the database.
group - groups the relation on the attribute supplied.
having - provides an expression limiting group relations (GROUP BY constraint).
joins - joins the relation to another table.
clause - provides an expression limiting join relations (JOIN constraint).
includes - includes other relations pre-loaded.
order - orders the relation based on the expression supplied.
limit - limits the relation to the number of records specified.
lock - locks the records returned from the table.
readonly - returns an read only copy of the data.
from - provides a way to select relationships from more than one table.
scope - (previously named_scope) return relations and can be chained together with the other relation methods.
with_scope - and with_exclusive_scope now also return relations and so can be chained.
default_scope - also works with relations.
```

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

returns an read only copy of the data.注意是数据库返回的结果就是只读，和 `attr_readonly` 是两码事。

Sets readonly attributes for the returned relation. If value is true (default), attempting to update a record will result in an error.

```ruby
users = User.readonly
users.first.save
=> ActiveRecord::ReadOnlyRecord: ActiveRecord::ReadOnlyRecord
```

`reorder(*args)`

重新排序。调用了其他人写的 query 方法，但对排序不满意的，可以重新排序。

`joins(*args)`

Active Record lets you use the names of the associations defined on the model as a shortcut for specifying JOIN clauses for those associations when using the joins method.

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

Even though Active Record lets you specify conditions on the eager loaded associations just like joins, the recommended way is to use joins instead.

However if you must do this, you may use where as you would normally.

```ruby
Post.includes(:comments).where("comments.visible" => true)
```

`distinct(value = true)`  aliased as: uniq

`explain()` 看看转化成 SQL 是什么样。

> Note: 注意区分 Rails 里的 group 和 SQL 里的 group_by

相关SQL [SQL Functions](http://www.w3schools.com/sql/sql_functions.asp)

参考官方文档 [Active Record Query Interface](http://edgeguides.rubyonrails.org/active_record_querying.html)
