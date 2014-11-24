# Engine full vs mountable

生成命令 `rails plugin new`

常用参数 `--full` 或 `--mountable`

## full

main_app 路由继承于 Engine，所以它们用的路由是同一套。

```ruby
# my_engine/config/routes.rb
Rails.application.routes.draw do
  # ...
end
```

因此，在 main_app 的 config/routes.rb 里不用做任何配置，它们是互通的。

main_app 会继承 Engine 的 model、controller、routes 等。

也就是说它们的环境是一样的。

## mountable

首先，从路由上讲它们是隔离开的：

```
# my_engine/config/routes.rb
MyEngine::Engine.routes.draw do
  # ...
end

# parent_app/config/routes.rb
ParentApp::Application.routes.draw do
  mount MyEngine::Engine => "/engine", :as => "namespaced"
end
```

其次，Engine 使用自己的命名空间：

```ruby
# my_engine/lib/my_engine/engine.rb
module MyEngine
  class Engine < Rails::Engine
    isolate_namespace MyEngine
  end
end
```

再者，Engine 创建的表有 engine_name 做为前缀。

model、controller、routes 等都是相互隔离的。

也就是说它们的环境不是一样的。

## 那么问题就来了

--full 仅做文件、目录上的分隔，实际上我们没必要使用，有其它方式实现(如：使用命名空间)。

--mountable 才是推荐做法(从 Engine 存在的意义及文档上，可以看出这点)。

其它更多信息，参考 [Rails Engine Patterns](http://www.slideshare.net/AndyMaleh/rails-engine-patterns)
