### 一般常用配置项

```ruby
# 自动加载目录
config.autoload_paths
# 立即加载目录
config.eager_load_paths

# 立即加载开关。开发环境 false，生产环境 true
# eager_load 始终是立即加载，它只影响 autoload 配置
config.eager_load

# 每次 HTTP 请求之间，是否重新加载环境(类、模块)
# 是，则类似 load; 否，则类似 require
# 影响控制台里的 reload! 效果
config.cache_classes

# 缓存存储方式
config.cache_store

config.console

config.disable_dependency_loading

config.encoding

config.exceptions_app

config.file_watcher

config.filter_parameters

# 网站是否强制启用 https
config.force_ssl

# log 格式处理
config.log_formatter
# debug、还是 info
config.log_level

config.logger

config.middleware

config.reload_classes_only_on_change

secrets.secret_key_base

# 请使用 `public_file_server.enabled`
config.serve_static_files

# session 存储方式及限制条件
config.session_store

# 时区，默认为 UTC
config.time_zone
```
