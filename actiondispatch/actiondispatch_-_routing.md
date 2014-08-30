# ActionDispatch - Routing
This is the first of three articles that will delve into the dark recesses of the routing code. It deals only with the implementation of the routing **DSL** (e.g., the part you use in config/routes.rb). The next two articles will deal with route **recognition**, and route **generation**, respectively.

## DSL

```ruby
ActionController::Routing::Routes.draw do |map|
  # ...
  map.connect ":controller/:action/:id"
end
```

That ActionController::Routing::Routes reference is rather misleading. It is not a class, but is simply a constant referring to an instance of ActionController::Routing::RouteSet. (You can see the instantiation occurring at the bottom of action_controller/routing.rb.) That one instance is in charge of managing the routes for the entire lifetime of the Ruby process.

```ruby
module ActionController
  module Routing
    # ...
    Routes = RouteSet.new
  end
end
```

------

The RouteSet#draw method is where the DSL magic all begins. If you look in the routing.rb code (around line 1113), it’s only three lines long, but what a significant three lines those are!

```ruby
def draw
  clear!
  yield Mapper.new(self)
  named_routes.install
end
```

That one method is the entry point for the entire routing DSL. First, any existing routes are removed from the collection (via the clear! method). Then, a new ActionController::Routing::RouteSet::Mapper instance is created and yielded to the block (where it is generally bound to the map variable in config/routes.rb). After all the routes have been “drawn” (or defined), any named routes are installed into ActionController::Base, so that the named routing helpers can be used by controller and view code (as foo_url and foo_path).

ActionController::Routing::RouteSet::Mapper is nearly trivial. It’s just a proxy that delegates to the RouteSet instance that created it. When you call map.connect, the work is delegated to RouteSet#add_route. When you create a named route, the method_missing hook on the Mapper redirects the call to RouteSet#add_named_route. If you are looking for ways to extend routing, take note: Mapper is what you need to extend in order to add to or change the routing DSL. For an example of how to extend it, take a look at action_controller/resources.rb. (That’s where the new RESTful routing options are defined.)


connect --> add_route
create a named route --> add_named_route

```ruby
def add_route(path, options = {})
  route = builder.build(path, options)
  routes << route
  route
end
```

```
def builder
  @builder ||= RouteBuilder.new
end
```


`segments_for_route_path`

```ruby
def segment_for(string)
  segment = case string
    when /\A:(\w+)/
      key = $1.to_sym
      case key
        when :controller then ControllerSegment.new(key)
        else DynamicSegment.new key
      end
    when /\A\*(\w+)/ then PathSegment.new($1.to_sym, :optional => true)
    when /\A\?(.*?)\?/
      returning segment = StaticSegment.new($1) do
        segment.is_optional = true
      end
    when /\A(#{separator_pattern(:inverted)}+)/ then StaticSegment.new($1)
    when Regexp.new(separator_pattern) then
      returning segment = DividerSegment.new($&) do
        segment.is_optional = (optional_separators.include? $&)
      end
  end
  [segment, $~.post_match]
end
```

Any time you add a route to this collection, a set of helper methods are automatically generated for that route and are added to an anonymous module, which is used to install the helpers into (e.g.) the **ActionController::Base** class. This installation only occurs when the NamedRouteCollection#install method is called, and that happens at the end of RouteSet#draw.

## recognition

`generate(options, recall={}, method=:generate)`

```ruby
named_route_name = options.delete(:use_route)
if named_route_name
  named_route = named_routes[named_route_name]
  options = named_route.parameter_shell.merge(options)
end
```

```ruby
options = options_as_params(options)
expire_on = build_expiry(options, recall)
```

```ruby
if !named_route && expire_on[:controller] && options[:controller] && options[:controller][0] != ?/
  old_parts = recall[:controller].split('/')
  new_parts = options[:controller].split('/')
  parts = old_parts[0..-(new_parts.length + 1)] + new_parts
  options[:controller] = parts.join('/')
end
```

```ruby
options[:controller] = options[:controller][1..-1] if options[:controller] && options[:controller][0] == ?/
merged = recall.merge(options)
```

```ruby
if named_route
  path = named_route.generate(options, merged, expire_on)
  raise RoutingError, "#{named_route_name}_url failed to generate from #{options.inspect}, expected: #{named_route.requirements.inspect}, diff: #{named_route.requirements.diff(options).inspect}" if path.nil?
  return path

else
  merged[:action] ||= 'index'
  options[:action] ||= 'index'

  controller = merged[:controller]
  action = merged[:action]

  raise RoutingError, "Need controller and action!" unless controller && action
```

```ruby
routes = routes_by_controller[controller][action][options.keys.sort_by { |x| x.object_id }]
```

```ruby
  routes.each do |route|
    results = route.send(method, options, merged, expire_on)
    return results if results
  end
end
```


```ruby
# map.connect "/foo/:action", :controller => "foo"
def generate_raw(options, hash, expire_on = {})
  path = begin
    if hash[:controller] == "foo"
      expired = false
      action_value = hash[:action] || "index"
      expired, hash = true, options if !expired && expire_on[:action]
      if action_value == "index"
        "/foo"
      else
        "/foo/#{CGI.escape(action_value.to_s)}"
      end
    end
  end
  [path, hash]
end
```

---------

```ruby
body = "expired = false\n#{generation_extraction}\n#{generation_structure}"
body = "if #{generation_requirements}\n#{body}\nend" if generation_requirements
```

```ruby
def generation_extraction
  segments.collect do |segment|
    segment.extraction_code
  end.compact * "\n"
end
```


```ruby  Segment#extraction_code
def extraction_code
  s = extract_value
  vc = value_check
  s << "\nreturn [nil,nil] unless #{vc}" if vc
  s << "\n#{expiry_statement}"
end
```

## generation

```ruby
def recognize(request)
  params = recognize_path(request.path, extract_request_environment(request))
  request.path_parameters = params.with_indifferent_access
  "#{params[:controller].camelize}Controller".constantize
end
```

## 描述

1. DSL --> Mapper(Base, Concerns, HttpHelpers, Resources, Scoping)
2. 既然维护着一张路由表，如何向表里添加规则？
3. 外部的 url 是如何识别并处理的？先预处理，然后对照路由表，然后转发给 Controller#action，再接下来就是我们熟悉的东西了。
4. 路由分类具名路由 和 匿名路由，内部可以调用具名路由(不调用，它就没意义了)。那么这些方法在哪定义的？
5. Rails 引入了 engine 的概念，涉及到 engine 的路由表是如何工作的？
**注意上面描述里的动词**

1 里对应着 mapper.rb 文件，及里面的各个子模块

2 由 add_route 完成，涉及一大堆的东西

3 里的转发给 Controller#action 这部分，由 Dispatcher 完成。通过 controller.action(action).call(env)，到具体的 Controller & action -- 常见的 rack 用法

4 里的定义方法，由 NamedRouteCollection 完成。它还代表了具名路由

```ruby
def define_url_helper(route, name, options)
  helper = UrlHelper.create(route, options.dup)

  @module.remove_possible_method name
  @module.module_eval do
    define_method(name) do |*args|
      helper.call self, args
    end
  end

  helpers << name
end

def define_named_route_methods(name, route)
  define_url_helper route, :"#{name}_path",
    route.defaults.merge(:use_route => name, :only_path => true)
  define_url_helper route, :"#{name}_url",
    route.defaults.merge(:use_route => name, :only_path => false)
end
```

4 内部调用并不一定总是通过 具体路由进行调用，例如：有时候我们会使用 link_to @record ... 这种情况，极端情况下会由路由这边生成 url，由 Generator 完成

5 涉及 engine 的路由部分，由 MountedHelpers 完成。

Journey 就是个打杂的，其它看得见和看不见的功能由它负责。


## 参考：

[Routing Walkthrough Part 1](http://railscasts.com/episodes/231-routing-walkthrough)
[Under the hood: route recognition in Rails](http://weblog.jamisbuck.org/2006/10/4/under-the-hood-route-recognition-in-rails)
[Under the hood: route generation in Rails](http://weblog.jamisbuck.org/2006/10/16/under-the-hood-route-generation-in-rails)

[Routing Walkthrough Part 1](http://railscasts.com/episodes/231-routing-walkthrough)

[Rails 3.2.8 Route 源码分析](http://ruby-china.org/topics/5895)
