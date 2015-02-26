## Enum

```ruby
definitions.each do |name, values|
  klass.singleton_class.send(:define_method, name.to_s.pluralize)

  define_method("#{name}=")
  define_method(name)

  define_method("#{name}_before_type_cast")

  pairs.each do |value, i| # pairs 约等于 values
    define_method("#{value}?")
    define_method("#{value}!")
    klass.scope value
```

假设，我们有 `status` 字段，那么使用 `enum` 方法会引入什么魔法：

```ruby
class Post < ActiveRecord::Base
  enum status: [ :active, :archived ]
end
```

1 同名类方法(复数形式)

```ruby
Post.statuses
# => {"active"=>0, "archived"=>1}
```


2 值(scope)

```ruby
Post.active
# => SELECT "posts".* FROM "posts"  WHERE "posts"."status" = 0
```

3 值?

```ruby
post.active?
=> false
```


4 值!

```ruby
post.active!

begin transaction
  UPDATE "posts" SET "status" = ?, "updated_at" = ? \n
  WHERE "posts"."id" = 2  [["status", 0],
                           ["updated_at", "2014-04-20 09:06:53.722202"]]
commit transaction
=> true
```

5 同名实例方法(get)

```ruby
post.status
=> "active"
```

6 同名实例方法=(set)

```ruby
post.status = "archived"
=> "archived"
```

7 和同名实例方法(get)作用一样

```ruby
post.status_before_type_cast
=> "archived"
```

> Note: 注意 enum 的字段必须是 integer 类型。<br>但除了保存数据库外，其它地方显示的却是我们定义的字段，也就是说它真正保存在数据库里的是 integer, 但平时我们使用，用的是 string 类型。
