## 各个实例方法

```
inspect # alias :to_s

# attr_accessor，Journey::Formatter 的实例对象
formatter
formatter=

# attr_accessor，Journey::Routes 的实例对象
set
set=

# attr_accessor，NamedRouteCollection 的实例对象
named_routes
named_routes=

# attr_accessor
default_scope
default_scope=

# attr_accessor，Journey::Router 的实例对象
router
router=

# attr_accessor
disable_clear_and_finalize
disable_clear_and_finalize=

# attr_accessor
# 也就是 default_resources_path_names，默认有：new 和 edit
resources_path_names
resources_path_names=

# attr_accessor
default_url_options
default_url_options=

# attr_accessor，默认是 ActionDispatch::Request
request_class
request_class=

routes # alias :set

draw

# 往变量 @append 和 @prepend 里塞东西
append
prepend

finalize!

# 消除 named_routes、set、formatter
clear!

dispatcher

# 包含了所有 Engine 提供的 helper 方法，和 main_app 方法。
mounted_helpers

# 新增 Engine 提供的 helper 方法。
# 用到了 MountedHelpers 和 RoutesProxy
define_mounted_helper

# 它其它是一个 Module
# 所以我们可以 include Rails.application.routes.url_helpers
# 包含了所有和 URL 有关的 helper 方法(也就是所有 x_url 和 x_path 方法)。
url_helpers

# 没有路由规则？
empty?

# build_path
# build_conditions
# @set.add_route
# named_routes[name]
add_route

extra_keys
generate_extras

# generate_extras 和 url_for 调用到
generate

# 我们在路由规则是可以带默认参数的
# 这在定义的时候就可以知道
# 这里的意思其实是：有默认参数吗？
optimize_routes_generation?

find_script_name # url_for 调用到

# 可谓是其它组件 url_for 方法的源头
# (当然，得用到路由方法才行)
url_for
path_for # 简单封装 url_for

# 入口
# 浏览器里输入的网址，先经它处理
# 关键部分是调用 @router.recognize
recognize_path
```

```
# 使用 Journey 处理我们提供的路由规则，得到 path 这部分
build_path


```

`draw` 直接对外暴露的接口，初始化 Mapper 对象。然后交还各个模块处理。
本质是：运用 Mapper，处理 config/routes.rb 里的代码。

`initialize`用到了 NamedRouteCollection， 内容是

```
self.named_routes = NamedRouteCollection.new
```

初始化了几个有意义的变量，如：named_routes、@set、@router、@formatter

`generate` 用到了 Generator，内容是

```
Generator.new(route_key, options, recall, self).generate
```

`dispatcher` 用到了 Dispatcher 内容是

```
Routing::RouteSet::Dispatcher.new(defaults)
```

