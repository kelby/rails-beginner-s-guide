## Store

`store(store_attribute, options = {})` 以 JSON(也可以理解为Hash) 的形式存储某字段。

举例，我们数据库里有 `name` 字段，我们想这样存储：

`name = { last_name: "Kelby", first_name: "Lee" }`

```ruby
class User < ActiveRecord::Base
  store :name, accessors: [ :last_name, :first_name ], coder: JSON
end

u = User.new(last_name: 'Kelby', first_name: 'Lee')
u.last_name                    # Accessor stored attribute
u.name[:last_name] = 'Denmark' # Any attribute, even if not specified with an accessor

# There is no difference between strings and symbols for accessing custom attributes
u.settings[:last_name]  # => 'Denmark'
u.settings['last_name'] # => 'Denmark'
```

`store_accessor(store_attribute, *keys)` 给已有 Hash 追加 key

```ruby
# Add additional accessors to an existing store through store_accessor
class User < ActiveRecord::Base
  store_accessor :name, :nickname
end
```

`stored_attributes` 查询一个字段有哪些可用的 key

```ruby
User.stored_attributes[:name] # [:last_name, :first_name, :nickname]
```

> Note: store 封装了 Serialization.serialize 方法
