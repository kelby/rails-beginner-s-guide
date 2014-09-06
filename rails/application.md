# Application

Application 继承于 Engine，负责协调整个启动过程，包括：配置、初始化。

## Configuration

除了和 Engine、Railtie 有一样的配置项外，它新增了自己的配置项，如：cache_classes、consider_all_requests_local、filter_parameters、logger 等。

```
attr_accessor :allow_concurrency, :asset_host, :assets, :autoflush_log,
              :cache_classes, :cache_store, :consider_all_requests_local, :console,
              :eager_load, :exceptions_app, :file_watcher, :filter_parameters,
              :force_ssl, :helpers_paths, :logger, :log_formatter, :log_tags,
              :railties_order, :relative_url_root, :secret_key_base, :secret_token,
              :serve_static_assets, :ssl_options, :static_cache_control, :session_options,
              :time_zone, :reload_classes_only_on_change,
              :beginning_of_week, :filter_redirect, :x

attr_writer :log_level
attr_reader :encoding
```

```
annotations
colorize_logging, colorize_logging=
database_configuration
encoding=
log_level
paths
session_store
```

## Initialization

Rails::Application is responsible for executing all railties and engines
initializers. It also executes some bootstrap initializers (check
Rails::Application::Bootstrap) and finishing initializers, after all the others
are executed (check Rails::Application::Finisher).

**Bootstrap**

~~Load environment hook~~  
Load active support  
Set eager load  
Initialize logger  
Initialize cache  
Initialize dependency mechanism  
~~Bootstrap hook~~

**Finisher**

Add generator templates  
Ensure autoload once paths as subset  
Add builtin route  
Build middleware stack  
Define main app helper  
Add to prepare blocks  
Run prepare callbacks  
Eager load!  
~~Finisher hook~~  
Set routes reloader hook  
Set clear dependencies hook
