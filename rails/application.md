# Application

Application 继承于 Engine，负责协调整个启动过程，包括：配置、初始化。

```
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

env_config

config_for
create
```

除了以上对外提供的接口外，它还有一些很有用的方法。但不属于对外提供接口，如：

```
initialize!
```

## Configuration

除了和 Engine、Railtie 有一样的配置项外，它新增了自己的配置项，如：cache_classes、consider_all_requests_local、filter_parameters、logger 等。

和我们的配置直接相关：

```ruby
Rails.configuration == Rails.application.config
 => true

Rails.configuration.class                      
 => Rails::Application::Configuration
```

有以下方法：

```
annotations
colorize_logging
database_configuration
log_level
paths
session_store
```

## Initialization

Application 负责执行所有 Railtie 和 Engine 的初始化任务。可分为前期准备任务 Bootstrap，和后期收尾任务 Finisher.

### Bootstrap

~~加载 environment hook~~  
加载 active support  
设置 eager load  
初始化 logger  
初始化 cache  
初始化 dependency mechanism  
~~Bootstrap hook~~

### Finisher

添加 generator templates  
Ensure autoload once paths as subset  
添加 builtin route  
构建 middleware stack  
**定义 main_app helper**  
添加 to prepare blocks  
运行 prepare callbacks  
Eager load!  
~~Finisher hook~~  
设置 routes reloader hook  
设置 clear dependencies hook

## Rails 应用启动过程

AppName.initialize!

run_initializers(group, self)

```ruby
initializers.tsort_each do |initializer|
  initializer.run(*args) if initializer.belongs_to?(group)
end


@initializers ||= self.class.initializers_for(self)

Collection.new(initializers_chain.map { |i| i.bind(binding) })

def initializers_chain
  initializers = Collection.new
  ancestors.reverse_each do |klass|
    next unless klass.respond_to?(:initializers)
    initializers = initializers + klass.initializers
  end
  initializers
end


def run(*args)
  @context.instance_exec(*args, &block)
end
```

**注意**：各样的钩子，如：

before_configuration  
before_eager_load  
before_initialize  
after_initialize

它们在 Railtie::Configuration 里定义，使用范围很广。

**注意** config/application.rb

里先 require  
再 config  
又有 eager_load

最后才是 initializer!
