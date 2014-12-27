## Application

Application 继承于 Engine，负责协调整个启动过程，包括：**配置、初始化**。

### 配置

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

### 初始化

Application 负责执行所有 Railtie 和 Engine 的初始化任务。可分为前期准备任务 Bootstrap，和后期收尾任务 Finisher.

#### Bootstrap 打前锋

~~加载 environment hook~~  
加载 active support  
设置 eager load  
初始化 logger  
初始化 cache  
初始化 dependency mechanism  
~~Bootstrap hook~~

#### Finisher 断后

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
