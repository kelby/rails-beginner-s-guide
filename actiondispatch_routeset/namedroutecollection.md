## ~~Named Route Collection~~

**向 Rails 其它组件提供路由相的关 x_path, x_url 方法。**

include Enumerable，所以看到很多同名方法也就不奇怪了。它们意义和使用方式雷同，不再一一解释。

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      def initialize(request_class = ActionDispatch::Request)
        self.named_routes = NamedRouteCollection.new
        # ...
      end
    end
  end
end
```

### 各个实例方法

```ruby
routes

url_helpers_module

route_defined?

helper_names

clear! & clear

add & []= # 主要作用，添加 x_url 和 x_path 辅助方法
get & []

names

path_helpers_module
```

除上述方法外，还有：

```
key?

each

length
```

另外，还有私有方法 `define_url_helper`<br> 我们使用的路由相关的 x_url 和 x_path 辅助方法就是由它而来的。

```ruby
def define_url_helper(mod, route, name, opts, route_key, url_strategy)
  helper = UrlHelper.create(route, opts, route_key, url_strategy)
  mod.module_eval do
    define_method(name) do |*args|
      # ...
    end
  end
end
```
### ~~Url Helper~~

非常底层的实现，主要对外接口 self.create

当调用 Named Route Collection 的私有方法 define_url_helper 时会用到，部分内容是：

```ruby
helper = UrlHelper.create(route, opts, route_key, url_strategy)
```

### 哪里调用它了？

1) `@set.add_route` 添加路由规则时有调用 named_routes[name]

2) 而 `named_routes` 就是 NamedRouteCollection 的实例对象：

```ruby
attr_accessor :named_routes

def initialize
  self.named_routes = NamedRouteCollection.new
  # ...
end
```

3) 并且 `[]=` 方法实际就是 add 方法，而 add 又调用了上面的 define_url_helper
