## Statement Cache

提供 `create` 类方法：

```ruby
cache = StatementCache.create(Book.connection) do |params|
  Book.where(name: "my book").where("author_id > 3")
end
```

通过它可以缓存某些数据库操作(和 Relation 有点类似)。

提供 `execute(params, klass, connection)` 实例方法：

```ruby
cache.execute([], Book, Book.connection)
```

前面缓存起来的操作，对应着一个对象。通过它运行里面真正要执行的内容。
