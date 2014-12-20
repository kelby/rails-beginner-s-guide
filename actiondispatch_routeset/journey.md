## ~~Journey~~

**重要组成部分：**

- Routes

- Router

- Formatter

**RouteSet 里调用到它的几个方法：**

initialize

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      attr_accessor :formatter, :set, :named_routes, :default_scope, :router
      attr_accessor :disable_clear_and_finalize, :resources_path_names
      attr_accessor :default_url_options, :request_class

      alias :routes :set

      def initialize(request_class = ActionDispatch::Request)
        self.named_routes         = NamedRouteCollection.new
        self.resources_path_names = self.class.default_resources_path_names
        self.default_url_options  = {}
        self.request_class        = request_class

        @append                     = []
        @prepend                    = []
        @disable_clear_and_finalize = false
        @finalized                  = false

        @set       = Journey::Routes.new
        @router    = Journey::Router.new @set
        @formatter = Journey::Formatter.new @set
      end
    end
  end
end
```

build_path

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      def build_path(path, ast, requirements, anchor)
        strexp = Journey::Router::Strexp.new(
            ast,
            path,
            requirements,
            SEPARATORS,
            anchor)

        pattern = Journey::Path::Pattern.new(strexp)

        builder = Journey::GTG::Builder.new pattern.spec

        # Get all the symbol nodes followed by literals that are not the
        # dummy node.
        symbols = pattern.spec.grep(Journey::Nodes::Symbol).find_all { |n|
          builder.followpos(n).first.literal?
        }

        # ... ...

        pattern
      end
    end
  end
end
```

call

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      def call(env)
        req = request_class.new(env)
        req.path_info = Journey::Router::Utils.normalize_path(req.path_info)
        @router.serve(req)
      end
    end
  end
end
```

recognize_path

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      def recognize_path(path, environment = {})
        method = (environment[:method] || "GET").to_s.upcase
        path = Journey::Router::Utils.normalize_path(path) unless path =~ %r{://}
        extras = environment[:extras] || {}

        env = Rack::MockRequest.env_for(path, {:method => method})

        req = request_class.new(env)
        @router.recognize(req) do |route, params|
          # ... ...
        end
      end
    end
  end
end
```

**路由已经定义，请求过来如何匹配？**

之前用的是正则匹配，例如 "/pictures/A12345" 可以匹配到：

```
get 'pictures/:id' => 'pictures#show', :constraints => { :id => /[A-Z]\d{5}/ }
```
 
引入 journey 后, 性能及其它各方面都有了很大的改善。
