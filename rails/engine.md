# Engine

`Rails::Engine` allows you to wrap a specific Rails application or subset of
functionality and share it with other applications or within a larger packaged application.
Since Rails 3.0, every `Rails::Application` is just an engine, which allows for simple
feature and application sharing.


Any `Rails::Engine` is also a `Rails::Railtie`, so the same
methods (like `rake_tasks` and +generators+) and configuration
options that are available in railties can also be used in engines.

## Creating an Engine

In Rails versions prior to 3.0, your gems automatically behaved as engines, however,
this coupled Rails to Rubygems. Since Rails 3.0, if you want a gem to automatically
behave as an engine, you have to specify an +Engine+ for it somewhere inside
your plugin's +lib+ folder (similar to how we specify a +Railtie+):

```
  # lib/my_engine.rb
  module MyEngine
    class Engine < Rails::Engine
    end
  end
```

Then ensure that this file is loaded at the top of your `config/application.rb`
(or in your +Gemfile+) and it will automatically load models, controllers and helpers
inside +app+, load routes at `config/routes.rb`, load locales at
`config/locales`, and load tasks at `lib/tasks`.

## Configuration

Besides the +Railtie+ configuration which is shared across the application, in a
`Rails::Engine` you can access `autoload_paths`, `eager_load_paths`
and `autoload_once_paths`, which, differently from a `Railtie`, are scoped to
the current engine.

```
  class MyEngine < Rails::Engine
    # Add a load path for this specific Engine
    config.autoload_paths << File.expand_path("../lib/some/path", __FILE__)

    initializer "my_engine.add_middleware" do |app|
      app.middleware.use MyEngine::Middleware
    end
  end
```

## Generators

You can set up generators for engines with `config.generators` method:

```
  class MyEngine < Rails::Engine
    config.generators do |g|
      g.orm             :active_record
      g.template_engine :erb
      g.test_framework  :test_unit
    end
  end
```

You can also set generators for an application by using `config.app_generators`:

```
  class MyEngine < Rails::Engine
    # note that you can also pass block to app_generators in the same way you
    # can pass it to generators method
    config.app_generators.orm :datamapper
  end
```

## Paths

Since Rails 3.0, applications and engines have more flexible path configuration (as
opposed to the previous hardcoded path configuration). This means that you are not
required to place your controllers at `app/controllers`, but in any place
which you find convenient.

For example, let's suppose you want to place your controllers in `lib/controllers`.
You can set that as an option:

```
  class MyEngine < Rails::Engine
    paths["app/controllers"] = "lib/controllers"
  end
```

You can also have your controllers loaded from both `app/controllers` and
`lib/controllers`:

```
  class MyEngine < Rails::Engine
    paths["app/controllers"] << "lib/controllers"
  end
```

The available paths in an engine are:

```
  class MyEngine < Rails::Engine
    paths["app"]                 # => ["app"]
    paths["app/controllers"]     # => ["app/controllers"]
    paths["app/helpers"]         # => ["app/helpers"]
    paths["app/models"]          # => ["app/models"]
    paths["app/views"]           # => ["app/views"]
    paths["lib"]                 # => ["lib"]
    paths["lib/tasks"]           # => ["lib/tasks"]
    paths["config"]              # => ["config"]
    paths["config/initializers"] # => ["config/initializers"]
    paths["config/locales"]      # => ["config/locales"]
    paths["config/routes.rb"]    # => ["config/routes.rb"]
  end
```

The `Application` class adds a couple more paths to this set. And as in your
`Application`, all folders under +app+ are automatically added to the load path.
If you have an `app/services` folder for example, it will be added by default.

## Endpoint

An engine can be also a rack application. It can be useful if you have a rack application that
you would like to wrap with +Engine+ and provide some of the +Engine+'s features.

To do that, use the +endpoint+ method:

```
  module MyEngine
    class Engine < Rails::Engine
      endpoint MyRackApplication
    end
  end
```

Now you can mount your engine in application's routes just like that:

```
  Rails.application.routes.draw do
    mount MyEngine::Engine => "/engine"
  end
```

## Middleware stack

As an engine can now be a rack endpoint, it can also have a middleware
stack. The usage is exactly the same as in `Application`:

```
  module MyEngine
    class Engine < Rails::Engine
      middleware.use SomeMiddleware
    end
  end
```

## Routes

If you don't specify an endpoint, routes will be used as the default
endpoint. You can use them just like you use an application's routes:

```
  # ENGINE/config/routes.rb
  MyEngine::Engine.routes.draw do
    get "/" => "posts#index"
  end
```

## Mount priority

Note that now there can be more than one router in your application, and it's better to avoid
passing requests through many routers. Consider this situation:

```
  Rails.application.routes.draw do
    mount MyEngine::Engine => "/blog"
    get "/blog/omg" => "main#omg"
  end
```

+MyEngine+ is mounted at `/blog`, and `/blog/omg` points to application's
controller. In such a situation, requests to `/blog/omg` will go through +MyEngine+,
and if there is no such route in +Engine+'s routes, it will be dispatched to `main#omg`.
It's much better to swap that:

```
  Rails.application.routes.draw do
    get "/blog/omg" => "main#omg"
    mount MyEngine::Engine => "/blog"
  end
```

Now, +Engine+ will get only requests that were not handled by +Application+.

## Engine name

There are some places where an Engine's name is used:

* routes: when you mount an Engine with <tt>mount(MyEngine::Engine => '/my_engine')</tt>,
  it's used as default <tt>:as</tt> option
* rake task for installing migrations <tt>my_engine:install:migrations</tt>

Engine name is set by default based on class name. For `MyEngine::Engine` it will be
`my_engine_engine`. You can change it manually using the `engine_name` method:

```
  module MyEngine
    class Engine < Rails::Engine
      engine_name "my_engine"
    end
  end
```

## Isolated Engine

Normally when you create controllers, helpers and models inside an engine, they are treated
as if they were created inside the application itself. This means that all helpers and
named routes from the application will be available to your engine's controllers as well.

However, sometimes you want to isolate your engine from the application, especially if your engine
has its own router. To do that, you simply need to call +isolate_namespace+. This method requires
you to pass a module where all your controllers, helpers and models should be nested to:

```
  module MyEngine
    class Engine < Rails::Engine
      isolate_namespace MyEngine
    end
  end
```

With such an engine, everything that is inside the +MyEngine+ module will be isolated from
the application.

Consider such controller:

```
  module MyEngine
    class FooController < ActionController::Base
    end
  end
```

If an engine is marked as isolated, +FooController+ has access only to helpers from +Engine+ and
`url_helpers` from `MyEngine::Engine.routes`.

The next thing that changes in isolated engines is the behavior of routes. Normally, when you namespace
your controllers, you also need to do namespace all your routes. With an isolated engine,
the namespace is applied by default, so you can ignore it in routes:

```
  MyEngine::Engine.routes.draw do
    resources :articles
  end
```

The routes above will automatically point to `MyEngine::ArticlesController`. Furthermore, you don't
need to use longer url helpers like `my_engine_articles_path`. Instead, you should simply use
`articles_path` as you would do with your application.

To make that behavior consistent with other parts of the framework, an isolated engine also has influence on
`ActiveModel::Naming`. When you use a namespaced model, like `MyEngine::Article`, it will normally
use the prefix "my_engine". In an isolated engine, the prefix will be omitted in url helpers and
form fields for convenience.

```
  polymorphic_url(MyEngine::Article.new) # => "articles_path"

  form_for(MyEngine::Article.new) do
    text_field :title # => <input type="text" name="article[title]" id="article_title" />
  end
```

Additionally, an isolated engine will set its name according to namespace, so
MyEngine::Engine.engine_name will be "my_engine". It will also set MyEngine.table_name_prefix
to "my_engine_", changing the MyEngine::Article model to use the my_engine_articles table.

## Using Engine's routes outside Engine

Since you can now mount an engine inside application's routes, you do not have direct access to +Engine+'s
`url_helpers` inside +Application+. When you mount an engine in an application's routes, a special helper is
created to allow you to do that. Consider such a scenario:

```
  # config/routes.rb
  Rails.application.routes.draw do
    mount MyEngine::Engine => "/my_engine", as: "my_engine"
    get "/foo" => "foo#index"
  end
```

Now, you can use the `my_engine` helper inside your application:

```
  class FooController < ApplicationController
    def index
      my_engine.root_url # => /my_engine/
    end
  end
```

There is also a `main_app` helper that gives you access to application's routes inside Engine:

```
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

```
  form_for([my_engine, @user])
```

This code will use `my_engine.user_path(@user` to generate the proper route.

== Isolated engine's helpers

Sometimes you may want to isolate engine, but use helpers that are defined for it.
If you want to share just a few specific helpers you can add them to application's
helpers in ApplicationController:

```
  class ApplicationController < ActionController::Base
    helper MyEngine::SharedEngineHelper
  end
```

If you want to include all of the engine's helpers, you can use #helper method on an engine's
instance:

```
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

```
  rake ENGINE_NAME:install:migrations
```

Note that some of the migrations may be skipped if a migration with the same name already exists
in application. In such a situation you must decide whether to leave that migration or rename the
migration in the application and rerun copying migrations.

If your engine has migrations, you may also want to prepare data for the database in
the `db/seeds.rb` file. You can load that data using the `load_seed` method, e.g.

```
  MyEngine::Engine.load_seed
```

## Loading priority

In order to change engine's priority you can use +config.railties_order+ in main application.
It will affect the priority of loading views, helpers, assets and all the other files
related to engine or application.

```
  # load Blog::Engine with highest priority, followed by application and other railties
  config.railties_order = [Blog::Engine, :main_app, :all]
```

---

## 对外提供的接口

```
app
call, config
eager_load!, endpoint, env_config
find
helpers, helpers_paths
inherited, isolate_namespace
load_console, load_generators, load_runner, load_seed, load_tasks
railties, routes

load_config_initializer
```