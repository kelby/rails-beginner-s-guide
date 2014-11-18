## Schema

继承于 Migration

常用的如：

```
define
```

对应的是 db/schema.rb 里的方法，举例：

```ruby
ActiveRecord::Schema.define(version: 20380119000001) do
  ...
end
```

其它方法：

```
migrations_paths
```

> Note: 它里面不支持非 migration 语句，也就是说直接让它执行 SQL 语句是不行的。
