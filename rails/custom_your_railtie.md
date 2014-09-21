# 定制自己的 Railtie

```
# your_railtie/railtie.rb
module YourRailtie
  class Railtie < Rails::Railtie
    # ... ...
  end
end
```

```
require 'your_railtie/railtie'
```

## 继承于 Rails::Railtie

继承于 Rails::Railtie，即可创建自己的 Railtie. 在 Rails 启动的时候，它也会被执行。

举例：

```ruby
# lib/my_gem/railtie.rb
module MyGem
  class Railtie < Rails::Railtie
  end
end

# lib/my_gem.rb
require 'my_gem/railtie' if defined?(Rails)
```

创建 Railtie，除了 lib/my_gem/railtie.rb 里继承于 Rails::Railtie 外，你不需要更改其它地方的代码。并不影响 my_gem 原来要做的事，也正如此，my_gem 也可以在 Rails 之外使用。

## initializer

继承于 Rails::Railtie 所以有 `initializer` 方法:

```ruby
class MyRailtie < Rails::Railtie
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

## config

Inside the Railtie class, you can access a config object which contains configuration
shared by all railties and the application:

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

## rake_tasks & generators

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

## 其它

```
abstract_railtie?
config, configure, console
generate_railtie_name, generators
inherited, instance
railtie_name, railtie_namespace, rake_tasks, respond_to_missing?, runner
subclasses
```
