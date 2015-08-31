### Preload, Eagerload, Includes 和 Joins 等

延迟加载，如 Relation，scope
预先加载，如 includes

#### N + 1

```ruby
# 一次查询
doctors = Physician.all

# N 次查询
doctor.patients.each do |patient|
  puts patient.name
end
```

#### includes

把关系表数据也查询出来。两个查询都要做，关联对象也需要放到内存。

```ruby
User.includes(:posts)

=> SELECT "users".* FROM "users"
=> SELECT "posts".* FROM "posts"  WHERE "posts"."user_id" IN (1, 2, 3)
```

```ruby
# 第一次调用，需要查询，花销大
# 目的：查询主表和关联表
User.includes(:posts).where('posts.desc = "ruby is awesome"').references(:posts)

# =>
SELECT "users"."id" AS t0_r0, "users"."name" AS t0_r1, "posts"."id" AS t1_r0,
       "posts"."title" AS t1_r1,
       "posts"."user_id" AS t1_r2, "posts"."desc" AS t1_r3
FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" = "users"."id"
WHERE (posts.desc = "ruby is awesome")

# 再次调用，不需要查询，花销为零
@customers = Customer.joins(:products).where("products.is_master = true")
```

特点，生成一条还是两条 SQL 查询语句，取决于写法。

可分为 2 种不同情况，和 preload 一样、和 earge_load 一样。

性能最慢。

通过中间表的话，默认也加载。

a.includes(:bs).where(bs.x ...) includes 只包含符合条件的 a 和 a 下面符合条件的 bs

#### joins

普通的查询条件，关联对象不会放到内存。

```ruby
User.joins(:posts)
=> SELECT "users".* FROM "users" INNER JOIN "posts"
   ON "posts"."user_id" = "users"."id"

# 第一次调用，需要查询，花销一般
# 目的：查询主表，关联表仅做为查询条件之一

posts = Post.joins(:comments)
# => Post Load (0.1ms)  SELECT "posts".* FROM "posts" INNER JOIN
     "comments" ON "comments"."commentable_id" = "posts"."id" AND
     "comments"."commentable_type" = 'Post'

# 再次调用，也需要查询，花销一般
posts.first.comments
# => Comment Load (0.2ms) SELECT "comments".* FROM "comments"
     WHERE "comments"."commentable_id" = ? AND "comments"."commentable_type" = ?
     [["commentable_id", 1], ["commentable_type", "Post"]]
```

特点，不会查询出关联表的数据，仅做为查询条件。

**复杂的 joins**

```ruby
# has_and_belongs_to_many
product has_and_belongs_to_many :devices

Product.joins("join devices_products on products.id = devices_products.product_id")
       .where(["devices_products.device_id = ?", params[:device_id]])

# has_many :through
has_many :catalogs_products
has_many :catalogs, :through => :catalogs_products
  
Product.joins(:catalogs_products).where(catalogs_products:
                                        {catalog_id: params[:catalog_id]})
```

#### preload

类似 includes 的子集。

```ruby
User.preload(:addresses)

=> SELECT "users".* FROM "users"
=> SELECT "addresses".* FROM "addresses"  WHERE "addresses"."user_id" IN (1, 2)
```
特点，SQL 查询语句始终是两条(单独的数据库查询)。

后续，不能以关联表做为查询条件。

性能最快。

通过中间表的话，默认也加载。

a.preload(:bs).where(bs.x ...) preload 包含符合条件的 a 和 a 下面所有的 bs

> Note: 实际应该是 a.joins(:bs).where(bs.x ...).preload(:bs)

#### eager_load

```ruby
User.eager_load(:posts)
=> SELECT "users"."id" AS t0_r0, "users"."name" AS t0_r1, ...
FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" =
"users"."id"
```

特点，SQL 查询语句始终只有一条(使用了 LEFT JOIN)。

性能中等。

default_scope 不起作用。

通过中间表的话，要明确指出才加载。

#### references

includes 后面的查询条件，用的是 "关联表.属性"，有时候 Rails 不能推断出这个'关联表'到底是哪个(可以 includes 多个关联表)，需要用 references 指明。

Rails 3 比较'宽松'，includes 有时候不指定 references 也可以工作。
Rails 4 比较'严格'，includes 需要指定 references 才能工作。

举例：

```ruby
User.includes(:posts).where("posts.name = 'foo'")
# => Doesn't JOIN the posts table, resulting in an error.

User.includes(:posts).where("posts.name = 'foo'").references(:posts)
# => Query now knows the string references posts, so adds a JOIN
```

参考

[3 ways to do eager loading (preloading) in Rails 3 & 4](http://blog.arkency.com/2013/12/rails4-preloading/)<br>
[eager loading in rails](http://codedecoder.wordpress.com/2014/07/23/eager-loading-eager_load-preload-includes/)
