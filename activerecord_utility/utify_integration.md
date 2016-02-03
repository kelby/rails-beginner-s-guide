## Integration

**实例方法：**`to_param`

默认，Rails 生成 URL 时用的是 `primary key`，也就是数据库里的 `id` 属性。例如：

```ruby
user = User.find_by(name: 'Phusion')
user_path(user)  # => "/users/1"
```

这对于 SEO 和人类识别都不是很友好。我们可以重写 `to_param` 方法，设置更友好的内容：

```ruby
class User < ActiveRecord::Base
  def to_param  # overridden
    name
  end
end

user = User.find_by(name: 'Phusion')
user_path(user)  # => "/users/Phusion"
```

**实例方法：**`cache_key(*timestamp_names)`

返回一个能够标识对象的字符串：

```ruby
Product.new.cache_key     # => "products/new"
Product.find(5).cache_key # => "products/5" (updated_at not available)
Person.find(5).cache_key  # => "people/5-20071224150000" (updated_at available)
```

当我们需要缓存的地方很多时，默认生成字符串的规则可能满足不了我们的需求。我们可以传递参数给它，用新的规则生成字符串：

```ruby
Person.find(5).cache_key(:updated_at, :last_reviewed_at)
```

**类方法：**`to_param`

和实例方法 `to_param` 类似，使用举例：

```ruby
class User < ActiveRecord::Base
  to_param :name
end

user = User.find_by(name: 'Fancy Pants')
user.id         # => 123
user_path(user) # => "/users/123-fancy-pants"
```
