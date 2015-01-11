## Counter Cache

按要求加减指定计数器的值、统计数目的加一、统计数目的减一、重置计数器的值。

```
update_counters(id, counters)

# 下面这两个方法基于 update_counters
increment_counter(counter_name, id)
decrement_counter(counter_name, id)

reset_counters(id, *counters)
```

这几条命令直接转化成 sql 语句，所以性能上要比普通的"给对象的计数器赋值，然后保存对象"要快，并且准确性得到了更高的保证。

之前没有统计数目，新增统计数目，或之前的统计数目存在错误，使用 reset_counters 你可以又快、又准确的得到统计数目。

`update_counters(id, counters)`

计数器实现。在它基础上通常有加一、减一操作，但也可以单独使用。
考虑到并发，这里并不只是Rails层面的get，然后set。而是在SQL层面"真正执行时"才增量加减(但没有用锁机制)。下面的加一、减一方法都是这样。

```ruby
# 注意：跟的是字段名
Post.update_counters 3, comments_count: +1

UPDATE "posts" SET "comments_count" = COALESCE("comments_count", 0) + 1 \n
WHERE "posts"."id" = 3

# 新增计数器: 原来没有使用计数器的，现在我们希望添加一个计数器。因为数据已经存在了，我们需要设置正确的数目。
# Post.update_counters 3, comments_count: post.comments.count
```

`increment_counter(counter_name, id)`

给 counter_name 字段进行加一。

`decrement_counter(counter_name, id)`

给 counter_name 字段进行减一。

`reset_counters(id, *counters)`

上文提到的"新增计数器"，也可用此方法实现。

```ruby
# 注意：跟的关系表名(可以是多个)，但不是字段名
Post.reset_counters(3, :comments)
  
SELECT  "posts".* FROM "posts"  WHERE "posts"."id" = ? LIMIT 1  [["id", 3]]
SELECT COUNT(*) FROM "comments"  WHERE "comments"."post_id" = ?  [["post_id", 3]]
UPDATE "posts" SET "comments_count" = 2 WHERE "posts"."id" = 3
```

基于SQL层面，重置(理解为校正，而不是归零)一个或多个计数器的值。计数器有时候会不准，特别是我们用来计数关联对象的个数，而自己又手动删除它们。

> Note: counter_cache 的 attribute 默认是 read_only
