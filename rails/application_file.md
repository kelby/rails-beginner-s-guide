### Application 文件下的内容

**实例方法：**

```ruby
# 封装上一级的同名方法
console
generators
rake_tasks
runner
initializer

isolate_namespace

key_generator
message_verifier

reload_routes!

initialized?

config_for

helpers_paths

find_root
```

```
env_config

secrets
```

**类方法：**

```
create
```

除了以上对外提供的接口外，它还有一些很有用的方法。但不属于对外提供接口，如：

```
initialize!
```

**明确使用到的其它类或模块**

```
require 'active_support/key_generator'
require 'active_support/message_verifier'
require 'rails/engine'

autoload :Bootstrap
autoload :Configuration
autoload :DefaultMiddlewareStack
autoload :Finisher
autoload :Railties
autoload :RoutesReloader
```