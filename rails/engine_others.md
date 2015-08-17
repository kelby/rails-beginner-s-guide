## 其它

#### mount as - 在 Engine 之外使用其路由

mount(挂载)后 Engine 和应用之间的路由仍然是独立的，你仍然不能在应用里直接使用 Engine 里面的路由。举例：

```ruby
# config/routes.rb
Rails.application.routes.draw do
  mount MyEngine::Engine => "/my_engine", as: "my_engine"
  get "/foo" => "foo#index"
end
```

Engine 外面，使用 `my_engine` 访问 Engine 里面的路由：

```ruby
class FooController < ApplicationController
  def index
    my_engine.root_url # => /my_engine/
  end
end
```

Engine 里面，使用 `main_app` 访问 Engine 外面的路由：

```ruby
module MyEngine
  class BarController
    def index
      main_app.foo_path # => /foo
    end
  end
end
```

`:as` 可选项默认使用的是 `engine_name`，所以通常你可以省略它。

还有一种情况，需要传递 engine_name 以便生成路由，举例：

```ruby
form_for([my_engine, @user])
```

这里生成的路由规则类似 `my_engine.user_path(@user)`

#### helper - Isolated engine's helpers

有时候，你的 Engine 是 Isolated，但你又想使用 Engine 里面定义的 helper，你可以引入某个模块：

```ruby
class ApplicationController < ActionController::Base
  helper MyEngine::SharedEngineHelper
end
```

或者，引入 Engine 里面所有的 helper 模块：

```ruby
class ApplicationController < ActionController::Base
  helper MyEngine::Engine.helpers
end
```

> Note: 这里引入的只是 helpers 目录下的文件，在 Controller 里定义，然后使用 helper_method 的方法不包含在内。

#### Migrations & seed data

Engine 也可以有自己的迁移文件，和普通应用一样，它们位于 `db/migrate` 下面。

你可以运行以下命令，将 Engine 里的迁移文件复制到应用里：

```ruby
rake ENGINE_NAME:install:migrations
```

Engine 也可以有自己的 seed 文件，它们位于 `db/seeds.rb` 下面。

你可以运行以下命令，将 Engine 里的 seed 文件复制到应用里：

```ruby
MyEngine::Engine.load_seed
```

#### railties_order

场景

```
- app
  - views
    - shared
      - _header.html.erb       <-- 实际渲染的却是这个
  - ...
- config
- ...
- vendors
  - plugins
    - myplugin
      - app
        - views
          - controller1
            - action1.html.erb <-- 在这里渲染
          - shared
            - _header.html.erb <-- 希望渲染的是这个

<%= render 'shared/header' %>
```

按照直观的理解，渲染的应该是第 2 个模板。但实际情况却不是，所以需要我们配置。

你可以使用 `config.railties_order` 改变 Engine 以及应用的加载顺序：

```ruby
# load Blog::Engine with highest priority, followed by application and other railties
config.railties_order = [Blog::Engine, :main_app, :all]
```

main_app 表示我们的项目本身，在 Application::Finisher 里定义，all 表示所有其它的 Railtie，在 Application::Configuration 里初始化时定义。

> Note: 上面的例子，你也可以用其它手段完成，如 namespace 等。

#### 迁移文件

```
rake my_engine:install:migrations
```

#### 提示

除了 initializer 外，rake_tasks 也会被复制到 main_app 里。所以，有时候你会看到提示：  
"Copy migrations from #{railtie_name} to application"
