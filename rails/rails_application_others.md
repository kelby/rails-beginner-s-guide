# Application 其它

## Default Middleware Stack

`build_stack`

构建默认的 middleware stack，在 Finisher 里有调用代码。

可以通过以下命令查看：

```ruby
Rails.application.send(:default_middleware_stack)
```

## Routes Reloader*

`Rails.application.reload_routes!`

仅是开发环境下，仅对这条命令的处理。(除此之外，无用)

```ruby
def reload!
  clear!
  load_paths
  finalize!
ensure
  revert
end
```

## 启动过程

The application is also responsible for setting up and executing the booting
process. From the moment you require "config/application.rb" in your app,
the booting process goes like this:

  1)  require "config/boot.rb" to setup load paths<br>
  2)  require railties and engines<br>
  3)  Define Rails.application as "class MyApp::Application < Rails::Application"<br>
  4)  Run config.before_configuration callbacks<br>
  5)  Load config/environments/ENV.rb<br>
  6)  Run config.before_initialize callbacks<br>
  7)  Run Railtie#initializer defined by railties, engines and application.
      One by one, each engine sets up its load paths, routes and runs its config/initializers/* files.<br>
  8)  Custom Railtie#initializers added by railties, engines and applications are executed<br>
  9)  Build the middleware stack and run to_prepare callbacks<br>
  10) Run config.before_eager_load and eager_load! if eager_load is true<br>
  11) Run config.after_initialize callbacks
