# 启动过程

1. config.ru
require ::File.expand_path('../config/environment',  __FILE__)

2. environment.rb
require File.expand_path('../application', __FILE__)

3. application.rb
require File.expand_path('../boot', __FILE__)

4. boot.rb
ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../../Gemfile', __FILE__)

5. Gemfile
gem ‘gem_name'

6. boot.rb
执行 bundle

7. application.rb
require ’something’ # like require ‘rails’

AppName::Application < Rails::Application

config your AppName

8. enviroment.rb
AppName::Application.initialize!

```ruby
    # Initialize the application passing the given group. By default, the
    # group is :default
    def initialize!(group=:default) #:nodoc:
      raise "Application has been already initialized." if @initialized
      run_initializers(group, self)
      @initialized = true
      self
    end
```

9. 默认的 Railtie，Engine, Application

10. 定制的 Railtie

Build the middleware stack and run to_prepare callbacks
(这样查看 middleware Rails.application.send :middleware 顺序从前到后)
（Rails.application.send :default_middleware_stack 这样是默认的)

Run config.before_eager_load and eager_load!

config.after_initialize

---

1)  require "config/boot.rb" to setup load paths

```ruby
require File.expand_path('../boot', __FILE__)
```

2)  require railties and engines

```ruby
# Pick the frameworks you want:
require 'active_record/railtie'
require 'action_controller/railtie'
require 'action_mailer/railtie'
# require "active_resource/railtie"
require 'sprockets/railtie'
# require "rails/test_unit/railtie"
require 'csv'
require 'roo'
```

3)  Define Rails.application as "class MyApp::Application < Rails::Application”

```ruby
module Console
  class Application < Rails::Application
    … …
  end
end
```

4)  Run config.before_configuration callbacks

```ruby
      # First configurable block to run. Called before any initializers are run.
      def before_configuration(&block)
        ActiveSupport.on_load(:before_configuration, yield: true, &block)
      end
```

5)  Load config/environments/ENV.rb

6)  Run config.before_initialize callbacks
     # Second configurable block to run. Called before frameworks initialize.
      def before_initialize(&block)
        ActiveSupport.on_load(:before_initialize, yield: true, &block)
      end
7)  Run Railtie#initializer defined by railties, engines and application.

    # Sends the initializers to the +initializer+ method defined in the
    # Rails::Initializable module. Each Rails::Application class has its own
    # set of initializers, as defined by the Initializable module.
    def initializer(name, opts={}, &block)
      self.class.initializer(name, opts, &block)
    end
8)  Custom Railtie#initializers added by railties,

engines and applications are executed:
Rails.application.initializers


```ruby
    initializer "action_mailer.logger" do
      ActiveSupport.on_load(:action_mailer) { self.logger ||= Rails.logger }
    end
```
9)  Build the middleware stack and run to_prepare callbacks

```ruby
      # Defines generic callbacks to run before #after_initialize. Useful for
      # Rails::Railtie subclasses.
      def to_prepare(&blk)
        to_prepare_blocks << blk if blk
      end
```
10) Run config.before_eager_load and eager_load! if eager_load is true

```ruby
    config.eager_load_paths += %W(
      #{config.root}/app/workers
      #{config.root}/app/mailers
      #{config.root}/lib
      #{config.root}/app/models/**/*
      #{config.root}/app/models/concerns
      #{config.root}/app/controllers/concerns
    )
```

11) Run config.after_initialize callbacks

```ruby
    config.after_initialize do
      if ActionMailer::Base.preview_path
        ActiveSupport::Dependencies.autoload_paths << ActionMailer::Base.preview_path
      end
    end
```

```ruby
Rails.application.send :middleware
 => #<ActionDispatch::MiddlewareStack:0x007f922d82fed0 @middlewares=[Rack::UTF8Sanitizer, Rack::Sendfile, ActionDispatch::Static, Rack::Lock, #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007f922b765510>, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActiveRecord::Migration::CheckPending, ActiveRecord::ConnectionAdapters::ConnectionManagement, ActiveRecord::QueryCache, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>

Rails.application.send :default_middleware_stack
 => #<ActionDispatch::MiddlewareStack:0x007f922de455e0 @middlewares=[Rack::Sendfile, ActionDispatch::Static, Rack::Lock, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>
```
---

启动过程基本上都在：

Rails::Application - application.rb

Rails::Railtie::Configuration - configuration.rb



