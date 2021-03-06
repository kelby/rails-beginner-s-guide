### Configuration

Railtie、Engine、Application 都有自己的 Configuration 模块。

由于 Ruby 的继承机制，我们常用的 `config` 可以看作是它们中任意一个的实例对象。所以，理论上来说，它们提供的接口 `config` 都可直接调用。

**对外提供接口：**

```ruby
attr_accessor :allow_concurrency, :asset_host, :assets, :autoflush_log,
              :cache_classes, :cache_store, :consider_all_requests_local, :console,
              :eager_load, :exceptions_app, :file_watcher, :filter_parameters,
              :force_ssl, :helpers_paths, :logger, :log_formatter, :log_tags,
              :railties_order, :relative_url_root, :secret_key_base, :secret_token,
              :serve_static_assets, :ssl_options, :static_cache_control,
              :session_options, :time_zone, :reload_classes_only_on_change,
              :beginning_of_week, :filter_redirect, :x

attr_writer :log_level
attr_reader :encoding
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

`paths` 除了 Engine 包含的文件、目录结构外，这里有：

```ruby
paths.add "config/database",    with: "config/database.yml"
paths.add "config/secrets",     with: "config/secrets.yml"
paths.add "config/environment", with: "config/environment.rb"
paths.add "lib/templates"
paths.add "log",                with: "log/#{Rails.env}.log"
paths.add "public"
paths.add "public/javascripts"
paths.add "public/stylesheets"
paths.add "tmp"
```

另，自定义的 Railtie 和自定义的 Engine，也可以对外提供 `config` 接口。
