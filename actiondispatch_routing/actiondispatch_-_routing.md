## Routing 概述：生成、存储、识别

**Routing 的概念**：

- 除 route_set.rb 外，routing 目录里的其它模块。
- 对外提供接口。

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

### 路由的生成、存储、识别

1) 路由对象的定义和调用方式：

```ruby
# 定义
module Rails
  class Engine < Railtie
    def routes
      @routes ||= ActionDispatch::Routing::RouteSet.new
      @routes.append(&Proc.new) if block_given? # 调用时，也可以追加路由规则
      @routes
    end
  end
end

# 调用方式
Rails.application.routes
Rails.application.routes { ... }
```

2) 调用路由对象，生成路由规则：

```ruby
Rails.application.routes.draw do
  # ... block 内容
end
```

3) 生成路由规则，本质是对 block 内容的求值：

```ruby
def draw(&block)
  # ...
  eval_block(block)
  # ...
  nil
end
```

4) 怎么求值呢？借助了 Mapper 的实例对象：

```ruby
def eval_block(block)
  # ...
  mapper = Mapper.new(self)
  # ...
  mapper.instance_exec(&block)
end
```

5) Mapper 的实例对象有什么内容？

```ruby
module ActionDispatch
  module Routing
    class Mapper
      # 这里 set = Rails.application.routes
      def initialize(set)
        @set = set
        @scope = Scope.new({ :path_names => @set.resources_path_names })
        @concerns = {}
        @nesting = []
      end
    end
  end
end
```

6) Mapper 实例对象有了，下一步就是执行 instance_exec. 也就是运行 block 里的各个方法。

7) block 里的各个方法，是在 Mapper 下面的各个模块里定义的：

```ruby
module ActionDispatch
  module Routing
    class Mapper
      # ...

      class Constraints < Endpoint
        # ...
      end

      class Mapping
        # ...
      end

      class Scope
        # ...
      end

      # ... 下面的各个模块
      
      module Base
        # ...
      end
      
      module HttpHelpers
        # ...
      end
      
      module Redirection
        # ...
      end
      
      module Scoping
        # ...
      end
      
      module Concerns
        # ...
      end
      
      module Resources
        # ...
      end

      include Base
      include HttpHelpers
      include Redirection
      include Scoping
      include Concerns
      include Resources
    end
  end
end
```

在这里，不同的路由规则会有对应的模块进行处理，具体可以在对应的各个章节查看。

8) 附：Mapper 的 ancestors

```
ActionDispatch::Routing::Mapper.ancestors
=> [ActionDispatch::Routing::Mapper,

 ActionDispatch::Routing::Mapper::Resources,
 ActionDispatch::Routing::Mapper::Concerns,
 ActionDispatch::Routing::Mapper::Scoping,
 ActionDispatch::Routing::Redirection,
 ActionDispatch::Routing::Mapper::HttpHelpers,
 ActionDispatch::Routing::Mapper::Base,

 # ...,
 ... ...]
```
