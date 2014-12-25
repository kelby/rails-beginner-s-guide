## 定制自己的 Engine

Engine = Ruby gem + Rails MVC stack elements.

### 一，创建自己的 Engine

可用命令 `rails plugin new` 创建自己的 Engine.
<br>
常用可选参数 `--full` 或 `--mountable`

两者之间的区别，可以参考【Engine full vs mountable】章节。

拆分一下，步骤大概如下：

1) 继承于 Rails::Engine，一般把它们放在 lib/ 目录下。

```ruby
# lib/my_engine.rb
module MyEngine
  class Engine < Rails::Engine
    # ... ...
  end
end
```

2) 在 config/application.rb
(或 Gemfile) 里加载本文件。

Engine 相关的 model、controller 和 helper 会被加载到 app/ 里，route 会被加载到 config/routes.rb, locale 会被加载到 config/locales, tasks 会被加载到 lib/tasks.

```ruby
# config/application.rb
require 'my_engine/engine'

# 或者

# Gemfile
gem 'my_engine', path: "/path/to/my_engine"
```

3) 在 routes.rb 里 mount MyEngine::Engine

```ruby
Rails.application.routes.draw do
  mount MyEngine::Engine => "/engine"
end
```

### 二，编写 my_engine/engine.rb  文件内容

每个 Engine 都会有自己的 engine.rb 文件。里面有自己的 Engine 类，它继承于 ::Rails::Engine

#### 常用方法之 config、initializer

在这文件里，你可以使用 config, initializer 等方法。这点和定制 Railtie 类似，但不同点是：当前 Engine 的配置和初始化，作用域仅限于当前 Engine.

```ruby
class MyEngine < Rails::Engine
  # 添加新的、额外的目录到 autoload_paths 里
  config.autoload_paths << File.expand_path("../lib/some/path", __FILE__)

  initializer "my_engine.add_middleware" do |app|
    app.middleware.use MyEngine::Middleware
  end
end
```

config 是个方法，但同时它也是 Configuration 的实例对象，所以你可以使用 `config.generators`:

```ruby
class MyEngine < Rails::Engine
  config.generators do |g|
    g.orm             :active_record
    g.template_engine :erb
    g.test_framework  :test_unit
  end
end
```

还可使用 `config.app_generators`:

```ruby
class MyEngine < Rails::Engine
  # note that you can also pass block to app_generators in the same way you
  # can pass it to generators method
  config.app_generators.orm :datamapper
end
```

#### 常用方法之 isolate_namespace

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

#### 常用方法之 paths

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

#### 常用方法之 endpoint

Engine 内容也可以是一个 Rack Application. 当你的代码本身是 Rack Application，而又想使用 Engine 的特性时，可以这么做：

1) 在自己定义的 Engine 里，使用 `endpoint`:

```ruby
module MyEngine
  class Engine < Rails::Engine
    # Engine 的内容就是 MyRackApplication
    endpoint MyRackApplication
  end
end
```

2) 和平常一样，在 route 里 `mount` 你的 Engine:

```ruby
Rails.application.routes.draw do
  mount MyEngine::Engine => "/engine"
end
```

#### 常用方法之 middleware

Engine 内容也可以是一个 Middleware. 当你的代码本身是 Middleware，而又想使用 Engine 的特性时，可以这么做：

```ruby
module MyEngine
  class Engine < Rails::Engine
    # Engine 的内容就是 SomeMiddleware
    middleware.use SomeMiddleware
  end
end
```

#### 常用方法之 engine_name

用几个场景可能会用到 engine name:

* routes: 当你使用 mount(MyEngine::Engine => '/my_engine')
* rake task: 当你使用 my_engine:install:migrations

Engine name 默认根据类名而来，如 `MyEngine::Engine` 对应有
`my_engine_engine`. 你可以使用 `engine_name` 进行自定义:

```ruby
module MyEngine
  class Engine < Rails::Engine
    engine_name "my_engine"
  end
end
```

### 三，编写内容

做了上述两步后，就是编写内容。该做什么事，做什么事；该完成什么功能，完成什么功能。

### 四，在 main_app 引入定制的 Engine

也就是：

在 config/application.rb (或 Gemfile) 里加载本文件。
<br>
在 routes.rb 里 mount MyEngine::Engine

本章开头已讲，不再重复。
