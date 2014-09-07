# Railite

Railtie 是 Rails 的核心部分之一。通过它，可以扩展和/或修改 Rails 的初始化程序。

Every major component of Rails (Action Mailer, Action Controller,
Action View and Active Record) is a Railtie. Each of
them is responsible for their own initialization. This makes Rails itself
absent of any component hooks, allowing other components to be used in
place of any of the Rails defaults.

每一个 Rails 组件(如：Action Mailer, Action Controller,
Action View 和 Active Record等)都属于 Railtie. 因为它们都需要自己的初始化程序。

Developing a Rails extension does _not_ require any implementation of
Railtie, but if you need to interact with the Rails framework during
or after boot, then Railtie is needed.

什么时候需要使用 Railtie? 当你的扩展符合下列情况时，可以考虑：

* creating initializers
* configuring a Rails framework for the application, like setting a generator
* adding <tt>config.*</tt> keys to the environment
* setting up a subscriber with ActiveSupport::Notifications
* adding rake tasks

## Creating your Railtie

To extend Rails using Railtie, create a Railtie class which inherits
from Rails::Railtie within your extension's namespace. This class must be
loaded during the Rails boot process.

The following example demonstrates an extension which can be used with or without Rails.

```ruby
# lib/my_gem/railtie.rb
module MyGem
  class Railtie < Rails::Railtie
  end
end

# lib/my_gem.rb
require 'my_gem/railtie' if defined?(Rails)
```

## Initializers

To add an initialization step from your Railtie to Rails boot process, you just need
to create an initializer block:

```ruby
class MyRailtie < Rails::Railtie
  initializer "my_railtie.configure_rails_initialization" do
    # some initialization behavior
  end
end
```

If specified, the block can also receive the application object, in case you
need to access some application specific configuration, like middleware:

```ruby
class MyRailtie < Rails::Railtie
  initializer "my_railtie.configure_rails_initialization" do |app|
    app.middleware.use MyRailtie::Middleware
  end
end
```

Finally, you can also pass <tt>:before</tt> and <tt>:after</tt> as option to initializer,
in case you want to couple it with a specific step in the initialization process.

## Configuration

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

## Loading rake tasks and generators

If your railtie has rake tasks, you can tell Rails to load them through the method
rake_tasks:

```ruby
class MyRailtie < Rails::Railtie
  rake_tasks do
    load "path/to/my_railtie.tasks"
  end
end
```

By default, Rails load generators from your load path. However, if you want to place
your generators at a different location, you can specify in your Railtie a block which
will load them during normal generators lookup:

```ruby
class MyRailtie < Rails::Railtie
  generators do
    require "path/to/my_railtie_generator"
  end
end
```

## Application and Engine

A Rails::Engine is nothing more than a Railtie with some initializers already set.
And since Rails::Application is an engine, the same configuration described here
can be used in both.

Be sure to look at the documentation of those specific classes for more information.
