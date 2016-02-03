## Secure Token

提供 `has_secure_token(attribute = :token)` 类方法

```ruby
class User < ActiveRecord::Base
  has_secure_token
  has_secure_token :auth_token
end
```

使用它可以给某些属性赋值 SecureRandom::base58 长度的字符串。

默认约定使用 `:token` 属性，并生成了对应的 `regenerate_token` 方法：

```ruby
user = User.new
user.save

user.token # => "pX27zsMN2ViQKta1bGfLmVJE"
user.auth_token # => "77TMHrHJFvFDwodq8w7Ev2m7"
```

内部实现：

通过 `before_create` 及属性自带的读写方法完成赋值操作；
<br>
不按照约定来，后续也可以更改属性名。可用元编程生成的 `regenerate_#{attribute}` 方法进行更改属性值：

```ruby
user.regenerate_token # => true
user.regenerate_auth_token # => true
```
