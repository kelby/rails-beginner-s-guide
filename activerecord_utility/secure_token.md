## Secure Token

提供 `has_secure_token` 类方法

```ruby
class User < ActiveRecord::Base
  has_secure_token
  has_secure_token :auth_token
end
```

使用它可以给某些属性赋值 SecureRandom::base58 长度的字符串。

```ruby
user = User.new
user.save

user.token # => "pX27zsMN2ViQKta1bGfLmVJE"
user.auth_token # => "77TMHrHJFvFDwodq8w7Ev2m7"

user.regenerate_token # => true
user.regenerate_auth_token # => true
```