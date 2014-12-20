## Secure Password

类方法：

```
has_secure_password(options = {})
```

实例方法：

```ruby
authenticate(unencrypted_password)

# 以下两方法和 attr_accessor 类似
password=(unencrypted_password)
password_confirmation=(unencrypted_password)
```

依赖于 `gem 'bcrypt'`，必须有 `password_digest` 属性(可以没有 `password` 属性)，使用参考：

```ruby
class User < ActiveRecord::Base
  has_secure_password validations: false
end

user = User.new(name: 'david', password: 'mUc3m00RsqyRe')
user.save
user.authenticate('notright')      # => false
user.authenticate('mUc3m00RsqyRe') # => user

user.password = 'mUc3m00RsqyRe'
user.save                          # => false, confirmation doesn't match
user.password_confirmation = 'mUc3m00RsqyRe'
user.save                          # => true
user.authenticate('notright')      # => false
user.authenticate('mUc3m00RsqyRe') # => user
```

使用 has_secure_password 后，还会自动帮我们添加校验：

```
validates_length_of :password

和

validates_confirmation_of :password
```

下面是 Rails 里面默认的加密、解密实现：

```ruby
require 'bcrypt'
# => true

cost = BCrypt::Engine.cost
# => 10

unencrypted_password = "password"
# => "password"

password_digest = BCrypt::Password.create(unencrypted_password, cost: cost)
# => "$2a$10$GGtvADq0jfb9E2wy4Nq0je1ZrJbJrsRSigwtBMlAAfV5dbAEgjt7C"

BCrypt::Password.new(password_digest) == unencrypted_password
# => true
```
> Note: BCrypt::Password.create 加密，BCrypt::Password.new 解密。
