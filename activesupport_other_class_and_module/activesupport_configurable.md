## Configurable

```
config

config_accessor
```

config 使用举例：

```ruby
require 'active_support/configurable'

class User
  include ActiveSupport::Configurable
end

user = User.new

# 写 config 对象
user.config.allowed_access = true
user.config.level = 1

# 读 config 对象
user.config.allowed_access # => true
user.config.level          # => 1
```

config_accessor 使用举例：

```ruby
class User
  include ActiveSupport::Configurable
  
  # 直接使用
  config_accessor :allowed_access
end

# 相关类方法
User.allowed_access # => nil
User.allowed_access = false
User.allowed_access # => false

user = User.new

# 相关实例方法
user.allowed_access # => false
user.allowed_access = true
user.allowed_access # => true

User.allowed_access # => false
```

config_accessor 使用举例：

```ruby
class User
  include ActiveSupport::Configurable

  # 带 instance_reader、instance_writer 参数
  config_accessor :allowed_access, instance_reader: false, instance_writer: false
  
  # 带 instance_accessor 参数
  config_accessor :allowed_access, instance_accessor: false

  # 带 block
  config_accessor :hair_colors do
    [:brown, :black, :blonde, :red]
  end
end
```
