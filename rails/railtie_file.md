## Railtie 文件下的内容

提供以下方法：

### initializer

继承于 Rails::Railtie 所以有 `initializer` 方法:

```ruby
class MyRailtie < Rails::Railtie
  # initializer 来源于 
  initializer "my_railtie.configure_rails_initialization" do
    # some initialization behavior
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

> Note: `initializer` 定义于 Rails::nitializable. 它还可以接受 :before 或 :after 做为参数。

### config

你可以使用 config 对象，它在所有 Railtie 和你的应用里是共用的。

```ruby
class MyRailtie < Rails::Railtie
  # Customize the ORM
  config.app_generators.orm :my_railtie_orm

  # Add a to_prepare block which is executed once in production
  # and before each request in development
  config.to_prepare do
    MyRailtie.setup!
  end
end
```

> Note: `config` 定义于 Configurable. delegate :config, to: :instance

### rake_tasks & generators

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

### 其它方法

提供方法：

```
config    # 名词
configure # 动词

# 动词，接 block
rake_tasks
console
runner
generators

railtie_name
railtie_namespace

instance
subclasses

abstract_railtie? # 默认是 Rails::Railtie、Rails::Engine 和 Rails::Application

generate_railtie_name
```

部分方法的解释，可以参考已有解释的方法，或参考 Engine 里的方法。
