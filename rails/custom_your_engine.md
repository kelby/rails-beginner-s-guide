# 定制自己的 Engine

```ruby
# your_engine/engine.rb
module YourEngine
  class Engine < Rails::Engine
    # ... ...
  end
end
```

```ruby
require 'your_engine/engine'
```

## 继承于 Engine

1 继承于 Rails::Engine，一般把它们放在 lib/ 目录下：

```ruby
# lib/my_engine.rb
module MyEngine
  class Engine < Rails::Engine
  end
end
```

2 在 config/application.rb
(或 Gemfile) 里加载本文件。

Engine 相关的 model、controller 和 helper 会被加载到 app/ 里，route 会被加载到 config/routes.rb, locale 会被加载到 config/locales, tasks 会被加载到 lib/tasks.

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

Since you can now mount an engine inside application's routes, you do not have direct access to `Engine`'s
`url_helpers` inside `Application`. When you mount an engine in an application's routes, a special helper is
created to allow you to do that. Consider such a scenario:

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

Note that the `:as` option given to mount takes the `engine_name` as default, so most of the time
you can simply omit it.

Finally, if you want to generate a url to an engine's route using
`polymorphic_url`, you also need to pass the engine helper. Let's
say that you want to create a form pointing to one of the engine's routes.
All you need to do is pass the helper as the first element in array with
attributes for url:

```ruby
form_for([my_engine, @user])
```

This code will use `my_engine.user_path(@user` to generate the proper route.

## helper - Isolated engine's helpers

Sometimes you may want to isolate engine, but use helpers that are defined for it.
If you want to share just a few specific helpers you can add them to application's
helpers in ApplicationController:

```ruby
class ApplicationController < ActionController::Base
  helper MyEngine::SharedEngineHelper
end
```

If you want to include all of the engine's helpers, you can use #helper method on an engine's
instance:

```ruby
class ApplicationController < ActionController::Base
  helper MyEngine::Engine.helpers
end
```

It will include all of the helpers from engine's directory. Take into account that this does
not include helpers defined in controllers with helper_method or other similar solutions,
only helpers defined in the helpers directory will be included.

## Migrations & seed data

Engines can have their own migrations. The default path for migrations is exactly the same
as in application: `db/migrate`

To use engine's migrations in application you can use rake task, which copies them to
application's dir:

```ruby
rake ENGINE_NAME:install:migrations
```

Note that some of the migrations may be skipped if a migration with the same name already exists
in application. In such a situation you must decide whether to leave that migration or rename the
migration in the application and rerun copying migrations.

If your engine has migrations, you may also want to prepare data for the database in
the `db/seeds.rb` file. You can load that data using the `load_seed` method, e.g.

```ruby
MyEngine::Engine.load_seed
```

## railties_order

I've my main project, and I want to implement part of it with differentes customizations, so I'm using Engine.

```
- app
  - views
    - shared
      - _header.html.erb     <-- This one is called
  - ...
- config
- ...
- vendors
  - plugins
    - myplugin
      - app
        - views
          - controller1
            - action1.html.erb
          - shared
            - _header.html.erb       <--- I want to render this!
But if from action1.html.erb I call

<%= render 'shared/header' %>
```

按照直观的理解，渲染的应该是第 2 个模板。但实际情况却不是，所以需要我们配置。

In order to change engine's priority you can use `config.railties_order` in main application.
It will affect the priority of loading views, helpers, assets and all the other files
related to engine or application.

```ruby
# load Blog::Engine with highest priority, followed by application and other railties
config.railties_order = [Blog::Engine, :main_app, :all]
```

main_app 表示我们的项目本身，在 Application::Finisher 里定义，all 表示所有其它的 Railtie，在 Application::Configuration 里初始化时定义。

> Note: 上面的例子，你也可以用其它手段完成，如 namespace 等。
