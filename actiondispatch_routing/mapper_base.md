### Base

常用方法：

```
match

mount

root
```

#### match 方法

这里的 `match` 只是个同名方法，是个空壳子，具体实现要看 Resources 里的 `match`.

另外，mount 和 root 本质上，都是封装和扩展 `match` 方法。

```
mount 和 root
      |
      V
    match
```

#### mount 方法

挂载一个基于 Rack 的应用到我们的程序。

```ruby
match '/movies/search', => "movies#search"
# 去掉语法糖，等价
match '/movies/search', => MoviesController.action(:search)
```

```ruby
# 这是一个 Rack. 所以可以调用 call 方法，传递 env 对象。
MoviesController.action(:search).call(env)

# 这也是一个 Rack.
Proc.new { |env|
  [
    200,
    {"Content-Type" => 'text/plain'},
    ["Hello, world"]
  ]
}

# 这还是一个 Rack.
class ApiApp
  def call(env)
    [
      200,
      {"Content-Type" => 'text/plain'},
      ["Hello, world"]
    ]
  end
end
```

```ruby
# 替换 Rack
match '/movies/search', => proc { |env|
                                  [
                                    200,
                                    {"Content-Type" => 'text/plain'},
                                    ["Hello, world"]
                                  ]
                                }
# 替换 Rack
match '/movies/search', => ApiApp
```

```ruby
# 进价
scope '/api' do
  match '(*path)', => ApiApp
end

# 加上语法糖，等价
mount ApiApp, :at => '/api'
# 或
mount ApiApp => "api"
```

因为 mount 实现基于 match，可以使用相同的可选参数。例如：

```ruby
mount(SomeRackApp => "some_route", as: "exciting")
```

现在，你可以通过 `exciting_path` 或 `exciting_url` 访问到刚才挂载的应用。

使用 `mount` 代替`match` 还有一个细节不同，在被挂载的 Rack endpoint 里，映射路由时我们不必加前缀。举例：

不是：

```ruby
require 'sinatra'
class ApiApp < Sinatra::Base
  get '/api' do
  end

  get "/api/endpoint" do
  end

  post "/api/endpoint" do
  end
end
```

而是：

```ruby
require 'sinatra'
class ApiApp < Sinatra::Base
  get '/' do
  end

  get "/endpoint" do
  end

  post "/endpoint" do
  end
end
```

#### root 方法

实现：

```ruby
def root(options = {})
  match '/', { :as => :root, :via => :get }.merge!(options)
end
```

因为 root 实现基于 match，可以使用相同的可选参数。

> 建议你把 `root` 放在 `config/routes.rb` 的开头部分，因为 Rails 的匹配规则是从上至下生成的，会优先匹配。
