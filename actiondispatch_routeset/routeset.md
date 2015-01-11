## 各个实例方法

在 Rails 内部，app.routes 就是 RouteSet 的实例对象。

```ruby
# action_mailer/railtie.rb
extend ::AbstractController::Railties::RoutesHelpers.with(app.routes, false)
include app.routes.mounted_helpers

app.routes.append do

# action_controller/railtie.rb
include app.routes.mounted_helpers
extend ::AbstractController::Railties::RoutesHelpers.with(app.routes)

# action_dispatch/routing/mapper.rb
app.routes.define_mounted_helper(name)
app.routes.extend Module.new {

# rails/application/finisher.rb
app.routes.append do
app.routes.define_mounted_helper(:main_app)
```

在 Rails 内容，@set 有时候也是 RouteSet 的实例对象，如 Mapper 里：

```ruby
# action_dispatch/routing/mapper.rb
initialize
  @set = set
  @scope = Scope.new({ :path_names => @set.resources_path_names })

dispatcher
  @set.dispatcher defaults

default_url_options=
  @set.default_url_options = options
  
has_named_route?
  @set.named_routes.routes[name.to_sym]
  
define_generate_prefix
  _route = @set.named_routes.get name
  _routes = @set
  
add_route
  mapping = Mapping.build(@scope, @set, URI.parser.escape(path), as, options)
  @set.add_route(app, conditions, requirements, defaults, as, anchor)
  
name_for_action
  candidate unless candidate !~ /\A[_a-z]/i || @set.named_routes.key?(candidate)
```

也正因为如此，除了 draw 和 add_routes 外，我们可以在代码里搜索，看哪里的代码影响了路由规则的生成。

```
attr_accessor :formatter, :set, :named_routes, :default_scope, :router
attr_accessor :disable_clear_and_finalize, :resources_path_names
attr_accessor :default_url_options, :request_class

# formatter 是 Journey::Formatter 的实例对象
# set 是 Journey::Routes 的实例对象
# named_routes 是 NamedRouteCollection 的实例对象
# router 是 Journey::Router 的实例对象
# resources_path_names 也就是 default_resources_path_names，默认有：new 和 edit
# request_class 默认是 ActionDispatch::Request
```

```
inspect & to_s
routes & set

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

# 会调用到以下方法：
# build_path
# build_conditions
# @set.add_route
# named_routes[name]
add_route

extra_keys
generate_extras

# generate_extras 和 url_for 调用到
generate

# 我们在路由规则是可以带默认参数的，这在定义的时候就可以知道。
# 这里的意思其实是：有默认参数吗？
optimize_routes_generation?

find_script_name # url_for 调用到

# 可谓是其它组件 url_for 方法的源头
# (当然，得用到路由方法才行)
url_for
path_for # 简单封装 url_for

# 浏览器里输入的网址，最先经过它解析
# 关键部分是调用 @router.recognize
recognize_path
```

`recognize_path` 使用举例：

```ruby
Rails.application.routes.recognize_path  "http://localhost:3000/users/1"
# => {:controller=>"users", :action=>"show", :id=>"1"}
```

通过这一步，外部 URL 已经被转换成了 Rails 能够识别的语言。在这之后，Rails 想怎么处理就怎么处理。

```
# 使用 Journey 处理我们提供的路由规则，得到 path 这部分
build_path
```

`draw` 直接对外暴露的接口，初始化 Mapper 对象。然后交还各个模块处理。
本质是：运用 Mapper，处理 config/routes.rb 里的代码。

相关代码：

```ruby
mapper = Mapper.new(self)

if default_scope
  mapper.with_default_scope(default_scope, &block)
else
  mapper.instance_exec(&block)
end
```

`initialize`用到了 NamedRouteCollection， 内容是：

```
self.named_routes = NamedRouteCollection.new
```

初始化了几个有意义的变量，如：named_routes、@set、@router、@formatter

`generate` 用到了 Generator，内容是：

```
Generator.new(route_key, options, recall, self).generate
```

`dispatcher` 用到了 Dispatcher 内容是：

```
Routing::RouteSet::Dispatcher.new(defaults)
```
