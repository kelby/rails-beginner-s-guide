### Environments

```
config.cache_classes = true

config.eager_load = true

config.consider_all_requests_local       = false

config.public_file_server.enabled = ENV['RAILS_SERVE_STATIC_FILES'].present?

config.log_level = :debug

config.log_formatter = ::Logger::Formatter.new
```

ache_classes = false 每一次 HTTP 请求之间，都会重新加载代码，类似于 load 方法。
而设置为 true 后，则只会在第一次加载，后续不加载，相当于 require.

```ruby
Rails.configuration.instance_variables

=> [
# 项目所在操作系统上的绝对路径
:@root, 

# :rails, :active_record, :test_unit 等
:@generators, 

# 用到的中间件
:@middleware, 

# 编码（默认是 utf-8）
:@encoding, 

# 允许并发？
:@allow_concurrency,

# 本地请求。决定了错误堆栈显示
:@consider_all_requests_local,

# 日志里过滤什么参数
:@filter_parameters,

# 日志里重定向过滤
:@filter_redirect, 

# 辅助文件所在路径
:@helpers_paths,

# 新项目默认首页，是否使用、使用哪个页面
:@public_file_server, 

# 强制 https 链接
:@force_ssl, 

# https 链接参数
:@ssl_options, 

# 怎么存储 session
:@session_store, 

# session 相关参数
:@session_options,

# 时区格式。默认用 UTC
:@time_zone, 

# 每周开始时间。默认是周一，比如有的是周日
:@beginning_of_week, 

# 日志
:@log_level, 

# 缓存怎么存、放在哪？
:@cache_store, 

# 默认为 all
:@railties_order,

:@relative_url_root, 

# 即使是开发环境，也应该只重新加载更改过的代码
:@reload_classes_only_on_change, 

# 文件更改检测
:@file_watcher, 

:@exceptions_app,

# 默认 true
:@autoflush_log, 

# 默认 Formatter
:@log_formatter, 

# 默认 true
:@eager_load, 

# 默认没有
:@secret_token, 

# 默认没有
:@secret_key_base,

# 是否只用 Rails API
:@api_only, 

# 默认 default
:@debug_exception_response_format, 

# 自己配置的，可以用这
:@x,

:@paths, 

# 自动加载
:@autoload_paths,

# 延迟加载
:@eager_load_paths, 

# 自动加载一次
:@autoload_once_paths, 

# 缓存类（代码）
:@cache_classes]
```

## 关于 autoload_paths 和 eager_load_paths

1. 我们需要 requie "x.rb" 才能运行
2. 我们并没有写 require
3. 开发环境，启动速度要快（不完整也没关系）
4. 生产环境，项目要完整（启动速度慢点也没关系）
5. 开发环境，经常更改代码，不用重启就能运行
6. 生产环境，假设不能/不会更改代码
7. 旧版本使用自动加载还面临线程安全问题，在这生产环境是不能忍受的

怎么自动加载？按约定来...

不按约定来呢？

app/ 既在 autoload_paths, 又在 eager_load_paths

```
# 开关
eager_load

# 立即加载此路径下的文件
eager_load_paths

# 自动(延迟)加载此路径下的文件
autoload_paths
```

什么情况下，你配置了也没用？

开发环境配置立即加载，仍然按自动加载执行。
<br>
生产环境下只配置自动加载，为了线程安全等考虑，需要再加上立即加载。

为什么不直接对 lib/ 用 eager_load_paths ？

想法：反正在开发环境是自动加载，在生产环境是立即加载。

坏处：用的是 lib/**/*.rb 模式，生产环境会把多余的 tasks、generators 也立即加载了。

require_dependency 如何使用？

initializers 和 tasks 只执行一次。

使用 eager_load_paths 会影响 autoload_paths。在开发环境，等同于使用 autoload_paths；在生产环境等同于它自己。

使用 autoload_paths 并不会影响 eager_load_paths

### 用 defined?(Blog) 检测

Foo::Bar::Baz 映射 foo/bar/baz.rb 所以你加载 lib/ 即可，不必具体到 lib/**/* 具体了反而不对！

## ActiveSupport::Dependencies.autoload_paths

## Rails 的坑

Rails 3、Rails 4 使用 autoload_paths 在生产环境也能工作，但偶尔会出问题。名字不按照约定，偶尔也会出问题。存在不确定性。

Rails 5 强制使用 eager_load_paths
