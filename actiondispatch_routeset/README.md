# ActionDispatch RouteSet

## class Dispatcher

每一个路由规则转换着 `draw` 对应一个 Dispatcher 实例。

重要的转发方法 `dispatch`，将战场切换到 Controller#action

实例方法

```ruby
dispatcher?
serve
prepare_params!
controller
```

私有方法

```ruby
controller_reference
dispatch
normalize_controller!
merge_default_action!
```

## class NamedRouteCollection

include Enumerable，所以看到很多同名方法也就不奇怪了。它们意义和使用方式雷同，不再一一解释。

### 各个实例方法

```ruby
routes
url_helpers_module
route_defined?
helpers
helper_names
clear! & clear
add 主要作用，添加 x_url 和 x_path 辅助方法
names

path_helpers_module
```

和 Enumerable 雷同的方法

```
get  针对的是 routes
key? 针对的是 routes
[]= 又名 add
[]  又名 get
each
length
```

另外，还有

`define_url_helper` 我们使用的路由相关的 x_url 和 x_path 辅助方法就是由它而来的。

### ~~UrlHelper~~

## ~~module MountedHelpers~~

## class Generator

各个实例方法

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

### 路由已经定义，请求过来如何匹配？

之前用的是正则匹配，例如
 `/pictures/A12345` 可以匹配到 `get 'pictures/:id' => 'pictures#show', :constraints => { :id => /[A-Z]\d{5}/ }`
 
后来引入了 journey, 性能及其它各方面都有了很大的改善。
