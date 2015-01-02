## ~~Routing 描述~~

路由三大块：DSL、recognition 和 generation.

### 描述

1. DSL --> Mapper(Base, Concerns, HttpHelpers, Resources, Scoping)
2. 既然维护着一张路由表，如何向表里添加规则？
3. 外部的 url 是如何识别并处理的？先预处理，然后对照路由表，然后转发给 Controller#action，再接下来就是我们熟悉的东西了。
4. 路由分类具名路由 和 匿名路由，内部可以调用具名路由(不调用，它就没意义了)。那么这些方法在哪定义的？
5. Rails 引入了 engine 的概念，涉及到 engine 的路由表是如何工作的？
**注意上面描述里的动词**

1) 里对应着 mapper.rb 文件，及里面的各个子模块。

2) 由 add_route 完成，涉及一大堆的东西。

3) 里的转发给 Controller#action 这部分，由 Dispatcher 完成。通过 controller.action(action).call(env)，到具体的 Controller & action -- 常见的 rack 用法。

4) 里的定义方法，由 Named Route Collection 完成。它还代表了具名路由。

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

4) 内部调用并不一定总是通过 具体路由进行调用，例如：有时候我们会使用 link_to @record ... 这种情况，极端情况下会由路由这边生成 url，由 Generator 完成。

5) 涉及 engine 的路由部分，由 Mounted Helpers 完成。

Journey 就是个打杂的，其它看得见和看不见的功能由它负责。

参考

[Routing Walkthrough Part 1](http://railscasts.com/episodes/231-routing-walkthrough)<br>
[Under the hood: route recognition in Rails](http://weblog.jamisbuck.org/2006/10/4/under-the-hood-route-recognition-in-rails)<br>
[Under the hood: route generation in Rails](http://weblog.jamisbuck.org/2006/10/16/under-the-hood-route-generation-in-rails)<br>
[Under the hood: Rails' routing DSL](http://weblog.jamisbuck.org/2006/10/2/under-the-hood-rails-routing-dsl)<br>
[Rails 3.2.8 Route 源码分析](http://ruby-china.org/topics/5895)<br>
[Rails 路由系统源码探索](https://ruby-china.org/topics/22726)
