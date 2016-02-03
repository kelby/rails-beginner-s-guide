## Enum

元编程生成一系列的方法：

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

假设，我们有 `status` 字段，使用 `enum` 后会生成什么方法？

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


2 与 value 同名的 scope 方法

```ruby
Post.active
# => SELECT "posts".* FROM "posts"  WHERE "posts"."status" = 0
```

3 value? 询问是否为某值

```ruby
post.active?
=> false
```


4 value! 更新为某值

```ruby
post.active!

begin transaction
  UPDATE "posts" SET "status" = ?, "updated_at" = ? \n
  WHERE "posts"."id" = 2  [["status", 0],
                           ["updated_at", "2014-04-20 09:06:53.722202"]]
commit transaction
=> true
```

5 同名实例方 法(get 类型)

```ruby
post.status
=> "active"
```

注意：此处这里如果用 `post[:status]` 的方式获取属性值，返回的值和数据库中保存的一样，是数字。

6 同名实例方法= (set 类型)

```ruby
post.status = "archived"
=> "archived"
```
注意：此处一定要区别于 `post[:status]=` 设置属性值。

7 同名实例方法(get 类型)

```ruby
post.status_before_type_cast
=> "archived"
```

注意 enum 的字段在数据库保存的是 integer 类型，但在外表现的却是字符串，我们查询、更新的都以字符串的形式进行。

另外：

注意 enum 生成的类方法、实例方法不要与 Active Record 提供的方法，及同一个 Model 下其它 enum 生成的方法有重复。

每个 Model 下面都会有一个叫 defined_enums 的变量用来记录 enum 相关信息。

读取某属性的值，通常有两种方式：

```ruby
user.name

# 或

user[:name]
```

一般情况下，它们的值是一样的。但这里的 Enum 打开了 `user.name` 方法，变换了返回结果，这会导致两者的值不一样。