## Application

Application 继承于 Engine，负责协调整个启动过程，包括：**配置、初始化**。

#### 配置

除了和 Engine、Railtie 有一样的配置项外，它新增了自己的配置项，如：cache_classes、consider_all_requests_local、filter_parameters、logger 等。

和我们的配置直接相关：

```ruby
Rails.configuration == Rails.application.config
 => true

Rails.configuration.class                      
 => Rails::Application::Configuration
```

#### 初始化

Application 负责执行所有 Railtie 和 Engine 的初始化任务。可分为前期准备任务 Bootstrap，和后期收尾任务 Finisher.
