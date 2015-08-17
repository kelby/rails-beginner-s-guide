## Secure Password

类方法：

```
has_secure_password(options = {})
```

实例方法：

```ruby
authenticate(unencrypted_password)

attr_reader :password

# 以下两方法和 attr_accessor 类似
password=(unencrypted_password)
password_confirmation=(unencrypted_password)
```

依赖于 `gem 'bcrypt'`，必须有 `password_digest` 属性(可以没有 `password` 属性)，使用参考：

```ruby
# Schema: User(name:string, password_digest:string)
class User < ActiveRecord::Base
  has_secure_password
end

user = User.new(name: 'david', password: '', password_confirmation: 'nomatch')
user.save                                       # => false, 密码不能为空
user.password = 'mUc3m00RsqyRe'
user.save                                       # => false, 确认密码失败
user.password_confirmation = 'mUc3m00RsqyRe'
user.save                                                       # => true
user.authenticate('notright')                                   # => false
user.authenticate('mUc3m00RsqyRe')                              # => user
User.find_by(name: 'david').try(:authenticate, 'notright')      # => false
User.find_by(name: 'david').try(:authenticate, 'mUc3m00RsqyRe') # => user
```

使用 has_secure_password 后，还会自动帮我们添加校验：

```ruby
validates_length_of       :password
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

# 加密
password_digest = BCrypt::Password.create(unencrypted_password, cost: cost)
# => "$2a$10$GGtvADq0jfb9E2wy4Nq0je1ZrJbJrsRSigwtBMlAAfV5dbAEgjt7C"

# 解密
BCrypt::Password.new(password_digest) == unencrypted_password
# => true
```
