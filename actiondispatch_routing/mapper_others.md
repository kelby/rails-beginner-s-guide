包含：Scope、Mapping、Constraints 等。

### 更多关于 Mapper

初始化的 Mapper 对象，包含：@set, @concerns, @nesting 和 @scope. 其中 @scope 为 Scope 实例对象。

之后运用它下面包含的各个子模块，各自处理。

### ~~Scope~~

让其它方法有 `@scope` 可用。

```ruby
module ActionDispatch
  module Routing
    class Mapper
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

### ~~Mapping~~

标准化路由规则。定义路由的方式有多种，所以，保存之前是有必要把它们标准化。

```ruby
module ActionDispatch
  module Routing
    class Mapper
      module Resources
        def add_route(action, options)
          path = path_for_action(action, options.delete(:path))

          # ... ...

          as = if !options.fetch(:as, true)
                 options.delete(:as)
               else
                 name_for_action(options.delete(:as), action)
               end

          mapping = Mapping.build(@scope, @set, URI.parser.escape(path), as, options)
          app, conditions, requirements, defaults, as, anchor = mapping.to_route

          @set.add_route(app, conditions, requirements, defaults, as, anchor)
        end
      end
    end
  end
end
```

### ~~Constraints~~

Endpoint 的子类之一，它是 endpoint.

```ruby
def app(blocks)
  if to.respond_to?(:call)
    Constraints.new(to, blocks, false)
  elsif blocks.any?
    Constraints.new(dispatcher(defaults), blocks, true)
  else
    dispatcher(defaults)
  end
end
```
