## ~~Dispatcher~~

**将 HTTP 请求向具体的 Controller#action 转移。**

继承于 Routing::Endpoint

```ruby
module ActionDispatch
  module Routing
    class RouteSet
      def dispatcher(defaults)
        Routing::RouteSet::Dispatcher.new(defaults)
      end
    end
  end
end
```

**实例方法：**

```ruby
dispatcher?

serve

prepare_params!

controller
```

**私有方法：**

```ruby
dispatch

controller_reference

normalize_controller!
merge_default_action!
```

最重要的方法是 `dispatch`，将战场切换到 Controller#action

```ruby
def dispatch(controller, action, env)
  controller.action(action).call(env)
end
```

**每一条路由规则，对应着一个 Dispatcher 实例。**

每一个路由规则转换着 `draw` 对应一个 Dispatcher 实例。

用最简单的 get 方法举例：

```ruby
AppName::Application.routes.draw do
  get 'photos/:id' => 'photos#show', :defaults => { :format => 'jpg' }
end
```

生成实例：

```ruby
app: #<ActionDispatch::Routing::RouteSet::Dispatcher:0x007fd05e0cf7e8
           @defaults={:format=>"jpg", :controller=>"photos", :action=>"show"},
           @glob_param=nil,
           @controller_class_names=#<ThreadSafe::Cache:0x007fd05e0cf7c0
           @backend={},
           @default_proc=nil>>
conditions: {:path_info => "/photos/:id(.:format)",
             :required_defaults => [:controller, :action],
             :request_method => ["GET"]}
requirements: {}
defaults: {:format=>"jpg", :controller=>"photos", :action=>"show"}
as: nil
anchor: true
```

`app` 如果条件匹配，将要执行的 Rack application.

`conditions` 匹配条件。执行此 Rack application 要匹配什么条件。

`defaults` 默认参数。

`requirements` 对应方法里的 :constraints 参数。

`as` 对应方法里的 :as 参数。
