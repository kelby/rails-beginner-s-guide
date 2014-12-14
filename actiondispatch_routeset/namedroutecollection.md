## Named Route Collection

**向 Rails 其它组件提供路由相关 helper 方法。**

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

另外，还有私有方法

`define_url_helper` 我们使用的路由相关的 x_url 和 x_path 辅助方法就是由它而来的。

### ~~Url Helper~~

非常底层的实现，主要对外接口 self.create

当调用 Named Route Collection 的私有方法 define_url_helper 时会用到，部分内容是

```
helper = UrlHelper.create(route, opts, route_key, url_strategy)
```
