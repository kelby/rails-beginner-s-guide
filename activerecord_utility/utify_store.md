## Store

`store(store_attribute, options = {})` 以 JSON(也可以理解为 Hash)的形式存储某字段。

举例，我们数据库里有 `name` 字段，我们想这样存储：

```
name = { last_name: "Kelby", first_name: "Lee" }
```

```ruby
class User < ActiveRecord::Base
  store :name, accessors: [ :last_name, :first_name ], coder: JSON
end

u = User.new(last_name: 'Kelby', first_name: 'Lee')
u.last_name               # 直接读/写 key
u.name[:last_name] = 'zk' # 通过 store 的属性来读/写 key

# 通过 store 的属性来读/写时，key 类型可以是 Symbol 或 String
u.settings[:last_name]  # => 'zk'
u.settings['last_name'] # => 'zk'
```

store 由【Serialization】的 `serialize` 和下面的 store_accessor 两方法组成。

`store_accessor(store_attribute, *keys)` 给已经存在数据的 store 添加 key.

```ruby
class User < ActiveRecord::Base
  store_accessor :name, :nickname
end
```

它主要是增加了和 key 同名的读/写(实例)方法。

`stored_attributes` 查询一个字段有哪些可用的 key.

```ruby
User.stored_attributes[:name] # [:last_name, :first_name, :nickname]
```

> Note: 通过 store 的属性来读/写 key，这里的 key 可以不在 accessors 范围里。如果不在范围里，则不能直接读/写 key，并且 stored_attributes 查看不到。
