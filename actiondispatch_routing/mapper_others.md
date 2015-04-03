包含：~~Scope、Mapping、Constraints~~

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
