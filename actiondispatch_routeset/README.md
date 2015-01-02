# Action Dispatch RouteSet

### RouteSet 概述

- 特指 route_set.rb 及 routes_proxy.rb
- 本身就充满魔法。
- 还是内外沟通的桥梁。
- 内指 Journey.
- 外指对外的接口及 routing 目录里的其它内容。

```ruby
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new

routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end
```

从 Action Dispatch 转换站场到 Action Controller.
(准确点：Action Dispatch -> Metal -> Abstract Controller -> Action Controller)

```ruby
def dispatch(controller, action, env)
  controller.action(action).call(env)
end
```

除上述外，还有：

```
Routes Proxy # 从 RouteSet 里抽取而来
```

### 包含以下几个部分

- 各个实例方法

- Dispatcher

- Named Route Collection

- Generator

- Journey

- Routes Proxy
