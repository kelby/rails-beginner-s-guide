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

 1) require "config/boot.rb" to setup load paths<br>
 2) require railties 和 engines<br>
 3) 定义 Rails.application as "class MyApp::Application < Rails::Application"<br>
 4) 运行 config.before_configuration callbacks<br>
 5) 加载 config/environments/ENV.rb<br>
 6) 运行 config.before_initialize callbacks<br>
 7) 运行 Railtie#initializer defined by railties, engines and application.
      One by one, each engine sets up its load paths, routes and runs its config/initializers/* files.<br>
 8) Custom Railtie#initializers added by railties, engines and applications are executed<br>
 9) 构建 the middleware stack and run to_prepare callbacks<br>
 10) 运行 config.before_eager_load and eager_load! if eager_load is true<br>
 11) 运行 config.after_initialize callbacks
