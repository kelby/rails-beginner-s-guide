## 各个实例方法

```
inspect
formatter
formatter=
set
set=
named_routes
named_routes=
default_scope
default_scope=
router
router=
disable_clear_and_finalize
disable_clear_and_finalize=
resources_path_names
resources_path_names=
default_url_options
default_url_options=
request_class
request_class=
routes
draw
append
prepend
finalize!
clear!
dispatcher
mounted_helpers
define_mounted_helper
url_helpers
empty?
add_route
extra_keys
generate_extras
optimize_routes_generation?
find_script_name
path_for
url_for
recognize_path
```

`draw` 直接对外暴露的接口。初始化 Mapper 对象，然后交还各个模块处理。
本质是：运用 Mapper，处理 config/routes.rb 里的代码。

`initialize`用到了 NamedRouteCollection， 内容是

```
self.named_routes = NamedRouteCollection.new
```

`generate` 用到了 Generator，内容是

```
Generator.new(route_key, options, recall, self).generate
```

`dispatcher` 用到了 Dispatcher 内容是

```
Routing::RouteSet::Dispatcher.new(defaults)
```

