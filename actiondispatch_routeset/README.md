# Action Dispatch RouteSet 路由底层（实现方式）

Routing 定义应用的路由后，由 RouteSet 底层实现。

本章节偏低层，如果你不能掌握，可以先跳过。

#### RouteSet 概述

* 特指 route\_set.rb 及 routes\_proxy.rb 两文件
* 本身就充满魔法，是 Routing 里的一个模块。
* 还是内外沟通的桥梁。
* 内指 Journey.
* 外指对外的接口及 routing 目录里的其它内容。

```ruby
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new

routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end
```

从 Action Dispatch 转换站场到 Action Controller.  
\(准确点：Action Dispatch -&gt; Metal -&gt; Abstract Controller -&gt; Action Controller\)

```ruby
# route_set.rb
def dispatch(controller, action, env)
  controller.action(action).call(env)
end
```

除上述外，还有：

```ruby
Routes Proxy # 从 RouteSet 里抽取而来
```



