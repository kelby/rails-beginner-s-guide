# Preload, Eagerload, Includes 和 Joins

## includes

把关系表数据也查询出来。

```ruby
User.includes(:posts)

=> SELECT "users".* FROM "users"
=> SELECT "posts".* FROM "posts"  WHERE "posts"."user_id" IN (1, 2, 3)
```

```ruby
User.includes(:posts).where('posts.desc = "ruby is awesome"').references(:posts)

# =>
SELECT "users"."id" AS t0_r0, "users"."name" AS t0_r1, "posts"."id" AS t1_r0,
       "posts"."title" AS t1_r1,
       "posts"."user_id" AS t1_r2, "posts"."desc" AS t1_r3
FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" = "users"."id"
WHERE (posts.desc = "ruby is awesome")

@customers = Customer.joins(:products).where("products.is_master = true")
```

特点，生成一条还是两条 SQL 查询语句，取决于写法。

## joins

```ruby
User.joins(:posts)
=> SELECT "users".* FROM "users" INNER JOIN "posts" ON "posts"."user_id" = "users"."id"
```

特点，不会查询出关联表的数据，仅做为查询条件。

## preload

类似 includes 的子集。

```ruby
User.preload(:addresses)

=> SELECT "users".* FROM "users"
=> SELECT "addresses".* FROM "addresses"  WHERE "addresses"."user_id" IN (1, 2)
```
特点，SQL 查询语句始终是两条。

## eager_load

```ruby
User.eager_load(:posts)
=> SELECT "users"."id" AS t0_r0, "users"."name" AS t0_r1, ...
FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" =
"users"."id"
```

特点，SQL 查询语句始终只有一条。

## 其它 references

includes 后面的查询条件，用的是 "关联表.属性"，有时候 Rails 不能推断出这个'关联表'到底是哪个，需要用 references 指明。

举例：

```ruby
User.includes(:posts).where("posts.name = 'foo'")
# => Doesn't JOIN the posts table, resulting in an error.

User.includes(:posts).where("posts.name = 'foo'").references(:posts)
# => Query now knows the string references posts, so adds a JOIN
```
