包含：Scope、Mapping、Constraints 等。

### 重中之重：match 和 scope 方法

从前面各章节，可归纳出路由规则里，重中之重是 match 和 cope 两方法。

`match` 是生成路由规则，`scope` 是影响如何生成路由规则。

和 match 有关的有：get, post, delete, put 等, 只有通过它们才能添加路由规则；
<br>
和 scope 有关的有：namescope, collection, member, resource, resouces 及嵌套路由时自动完成的 nested 等。它们主要工作是调用上面的 match 相关方法，并影响生成路由规则。

完成路由规则整个任务，match 和 scope 缺一不可。

此外，是嵌套、语法糖、去重复代码等。

### ~~Scope~~

`@scope` 是其实例对象。

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

标准化路由规则。

match 会调用 add_route，进而 @set.add_route 完成添加路由规则。但在 @set.add_route 之前，要先把路由规则标准化。

```ruby
module ActionDispatch
  module Routing
    class Mapper
      module Resources
        # ...
        
        def match
          # ...
        end
        
        # ... ...

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

标准化路由规则这个过程中，涉及到的一个对象。

```ruby
module ActionDispatch
  module Routing
    class Mapper
      class Mapping
        def to_route
          [ app(@blocks), conditions, requirements, defaults, as, anchor ]
        end

        private
        def app(blocks)
          if to.respond_to?(:call)
            Constraints.new(to, blocks, false)
          elsif blocks.any?
            Constraints.new(dispatcher(defaults), blocks, true)
          else
            dispatcher(defaults)
          end
        end
      end
    end
  end
end
```

Endpoint 的子类之一，它是 endpoint.
