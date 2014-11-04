# 定制自己的 Engine

Engine = Ruby gem + MVC stack elements

生成命令 `rails plugin new`

常用参数 `--full` 或 `--mountable`

## 继承于 Engine

1 继承于 Rails::Engine，一般把它们放在 lib/ 目录下：

```ruby
# lib/your_engine.rb
module YourEngine
  class Engine < Rails::Engine
    # ... ...
  end
end
```

2 在 config/application.rb
(或 Gemfile) 里加载本文件。

Engine 相关的 model、controller 和 helper 会被加载到 app/ 里，route 会被加载到 config/routes.rb, locale 会被加载到 config/locales, tasks 会被加载到 lib/tasks.

```ruby
# config/application.rb
require 'your_engine/engine'

# 或者

# Gemfile
gem 'your_engine', path: "/path/to/your_engine"
```

3 在 routes.rb 里 mount Your::Engine

## config 和 initializer

和 Railtie 一样，你可以使用 config, initializer 方法。但不同点是，在 Engine 里的配置和初始化，它们的作用域仅限于当前 Engine.

```ruby
class MyEngine < Rails::Engine
  # Add a load path for this specific Engine
  config.autoload_paths << File.expand_path("../lib/some/path", __FILE__)

  initializer "my_engine.add_middleware" do |app|
    app.middleware.use MyEngine::Middleware
  end
end
```

通过 config 方法，你还有 `config.generators` 方法:

```ruby
class MyEngine < Rails::Engine
  config.generators do |g|
    g.orm             :active_record
    g.template_engine :erb
    g.test_framework  :test_unit
  end
end
```

或者用 `config.app_generators` 方法:

```ruby
class MyEngine < Rails::Engine
  # note that you can also pass block to app_generators in the same way you
  # can pass it to generators method
  config.app_generators.orm :datamapper
end
```

## paths

Engine 默认都有自己的文件、目录结构，如果你没有定制，那么就使用默认的：

```
"app",                 eager_load: true, glob: "*"
"app/assets",          glob: "*"
"app/controllers",     eager_load: true
"app/helpers",         eager_load: true
"app/models",          eager_load: true
"app/mailers",         eager_load: true
"app/views"

"app/controllers/concerns", eager_load: true
"app/models/concerns",      eager_load: true

"lib",                 load_path: true
"lib/assets",          glob: "*"
"lib/tasks",           glob: "**/*.rake"

"config"
"config/environments", glob: "#{Rails.env}.rb"
"config/initializers", glob: "**/*.rb"
"config/locales",      glob: "*.{rb,yml}"
"config/routes.rb"

"db"
"db/migrate"
"db/seeds.rb"

"vendor",              load_path: true
"vendor/assets",       glob: "*"
```

`paths` 通过它，可以更改默认的文件、目录结构。

举例，你想把 controller 文件放到 lib/ 目录下：

```ruby
class MyEngine < Rails::Engine
  paths["app/controllers"] = "lib/controllers"
end
```

再或者，controller 在 app/ 和 lib/ 下都可接受：

```ruby
class MyEngine < Rails::Engine
  paths["app/controllers"] << "lib/controllers"
end
```

Application 在 Engine 之上，它又有自己的配置和初始化。它配置了 app/ 下的文件、目录会被自动加载，所以像 app/services 会被自动加载。

## Engine 内容是 Rack Application

Engine 内容也可以是一个 Rack Application. 当你的代码本身是 Rack Application，而又想使用 Engine 的特性时，可以这么做：

1 在自己定义的 Engine 里，使用 `endpoint`:

```ruby
module MyEngine
  class Engine < Rails::Engine
    # Engine 的内容就是 MyRackApplication
    endpoint MyRackApplication
  end
end
```

2 和平常一样，在 route 里 `mount` 你的 Engine:

```ruby
Rails.application.routes.draw do
  mount MyEngine::Engine => "/engine"
end
```

## Engine 内容是 Middleware

Engine 内容也可以是一个 Middleware. 当你的代码本身是 Middleware，而又想使用 Engine 的特性时，可以这么做：

```ruby
module MyEngine
  class Engine < Rails::Engine
    # Engine 的内容就是 SomeMiddleware
    middleware.use SomeMiddleware
  end
end
```

## Engine 使用自己的 Routes*

Engine 默认没有自己的 endpoint(入口)，使用 Engine 的应用用什么，它就用什么。需要的话，你也可以指定：

```ruby
# ENGINE/config/routes.rb
MyEngine::Engine.routes.draw do
  get "/" => "posts#index"
end
```

## engine_name

用几个场景可能会用到 engine name:

* routes: 当你使用 <tt>mount(MyEngine::Engine => '/my_engine')</tt>
* rake task: 当你使用 <tt>my_engine:install:migrations</tt>

Engine name 默认根据类名而来，如 `MyEngine::Engine` 对应有
`my_engine_engine`. 你可以使用 `engine_name` 进行自定义:

```ruby
module MyEngine
  class Engine < Rails::Engine
    engine_name "my_engine"
  end
end
```

## isolate_namespace

默认 Engine 和应用是在一个环境里的，这意味着应用所有 helper 和命名路由都可以在 Engine 里使用。

你可以使用 `isolate_namespace` 更改此项默认配置，将 Engine 和应用隔离出来。使用举例：

```ruby
module MyEngine
  class Engine < Rails::Engine
    isolate_namespace MyEngine
  end
end
```

此时 `MyEngine` 和应用是隔离了的，假设 MyEngine 有代码：

```
module MyEngine
  class FooController < ActionController::Base
  end
end
```

此时 +FooController+ 仅能使用 `Engine` 里提供的 helper，以及 `MyEngine::Engine.routes` 提供的 url helper.

另外一个改变就是 Engine 里的路由不必再使用 namespace，举例：

```ruby
MyEngine::Engine.routes.draw do
  resources :articles
end
```

resources :articles 自动对应着 `MyEngine::ArticlesController`. 并且不必使用长长的 url helper，例如 `my_engine_articles_path` 可以直接使用 `articles_path`

不受 isolate_namespace 影响的就是对于 model 的调用，仍然使用 engine_name 做为前缀。例如以下例子的 `MyEngine::Article`

```ruby
polymorphic_url(MyEngine::Article.new) # => "articles_path"

form_for(MyEngine::Article.new) do
  text_field :title # => <input type="text" name="article[title]" id="article_title" />
end
```

另一个改变是对表名的更改。默认使用 engine_name (在这里是 "my_engine")做为表前缀，也就是说 MyEngine::Article 对应的表名应该是 my_engine_articles

## mount as - 在 Engine 之外使用其路由

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

## helper - Isolated engine's helpers

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

## Migrations & seed data

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

## railties_order

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

## 迁移文件

```
rake my_engine:install:migrations
```
