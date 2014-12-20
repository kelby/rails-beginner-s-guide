## Configurable

### 实例方法

`config`
用 @_config 实例变量来保存配置信息。

使用举例：

```ruby
require 'active_support/configurable'

class User
  include ActiveSupport::Configurable
end

user = User.new

user.config.allowed_access = true
user.config.level = 1

user.config.allowed_access # => true
user.config.level          # => 1
```

### 类方法

`config_accessor`
以声明的形式，同时定义类方法和实例方法。

使用举例：

```ruby
class User
  include ActiveSupport::Configurable
  config_accessor :allowed_access
end

User.allowed_access # => nil
User.allowed_access = false
User.allowed_access # => false

user = User.new

user.allowed_access # => false
user.allowed_access = true
user.allowed_access # => true
User.allowed_access # => false
```

除此之外，还有：

```
config

configure
```

### Configuration

```
# 类方法
compile_methods!

# 实例方法
compile_methods!
```
