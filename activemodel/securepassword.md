## SecurePassword

较独立，有时候压根我们就不用它。

类方法：`has_secure_password(options = {})`

实例方法：

```ruby
authenticate(unencrypted_password)
password=(unencrypted_password)
password_confirmation=(unencrypted_password)
```

依赖于 `gem 'bcrypt'`，需要 `password` 和 `password_digest` 属性，使用参考：

```ruby
class User < ActiveRecord::Base
  has_secure_password validations: false
end

user = User.new(name: 'david', password: 'mUc3m00RsqyRe')
user.save
user.authenticate('notright')      # => false
user.authenticate('mUc3m00RsqyRe') # => user

user.password = 'mUc3m00RsqyRe'
user.save                                             # => false, confirmation doesn't match
user.password_confirmation = 'mUc3m00RsqyRe'
user.save                                             # => true
user.authenticate('notright')                         # => false
user.authenticate('mUc3m00RsqyRe')                    # => user
```

下面是 Rails 里面默认的加密、解密实现：

```ruby
[1] pry(main)> require 'bcrypt'
=> true
[2] pry(main)> cost = BCrypt::Engine.cost
=> 10
[3] pry(main)> unencrypted_password = "password"
=> "password"
[4] pry(main)> password_digest = BCrypt::Password.create(unencrypted_password, cost: cost)
=> "$2a$10$GGtvADq0jfb9E2wy4Nq0je1ZrJbJrsRSigwtBMlAAfV5dbAEgjt7C"
[5] pry(main)> BCrypt::Password.new(password_digest) == unencrypted_password
=> true
```
> **Note:** BCrypt::Password.create 加密，BCrypt::Password.new 解密
