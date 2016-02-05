## Railtie 文件下的内容

提供以下实例方法：

#### initializer

实际上 `initializer` 定义于 Rails::initializable. 它还可以接受 :before 或 :after 做为参数。

Rails::Railtie include 了它，所以对外提供有 `initializer` 方法:

```ruby
class MyRailtie < Rails::Railtie
  # initializer 来源于 Rails::initializable
  initializer "my_railtie.configure_rails_initialization" do
    # 一些初始化代码
  end
end
```

`initializer` 还可将应用做为参数，所以可以这么用:

```ruby
class MyRailtie < Rails::Railtie
  initializer "my_railtie.configure_rails_initialization" do |app|
    app.middleware.use MyRailtie::Middleware
  end
end
```

#### config

你可以使用 `config` 对象，它在所有 Railtie 和你的应用里是共用的。

```ruby
class MyRailtie < Rails::Railtie
  # 配置使用什么 ORM
  config.app_generators.orm :my_railtie_orm

  # to_prepare 里的内容在生产环境上只执行一次，在开发环境下每个请求都会请求一遍。
  config.to_prepare do
    MyRailtie.setup!
  end
end
```

> Note: `config` 定义于 Configurable. delegate :config, to: :instance

#### rake_tasks & generators

继承于 Rails::Railtie 所以有 `rake_tasks` 方法:

```ruby
class MyRailtie < Rails::Railtie
  rake_tasks do
    load "path/to/my_railtie.tasks"
  end
end
```

继承于 Rails::Railtie 所以有 `generators` 方法:

```ruby
class MyRailtie < Rails::Railtie
  generators do
    require "path/to/my_railtie_generator"
  end
end
```

#### 其它

类方法：

```ruby
# 动词，接 block
rake_tasks
console
runner
generators
```

```ruby
configure # 动词

railtie_name

instance
subclasses

abstract_railtie? # 默认是 Rails::Railtie、Rails::Engine 和 Rails::Application
```

实例方法：

```ruby
config # 名词
configure
railtie_namespace
```

部分方法的解释，可以参考已有解释的方法，或参考 Engine 里的方法。
