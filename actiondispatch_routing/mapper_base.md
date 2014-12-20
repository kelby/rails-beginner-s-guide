### Base

常用方法：

```
match
mount
root
```

#### match

匹配url到一个或多个路由。所有符号，都会对应着url里的参数，可用`params`获取：

```ruby
# 对应着 params 里的 :controller, :action 和 :id
match ':controller/:action/:id'
```

预留了两个符号，`:controller` 对应着 Controller，`:action` 对应着 action. 也可以接受模式匹配做为参数：

```ruby
match 'songs/*category/:title', to: 'songs#show'

# 'songs/rock/classic/stairway-to-heaven' sets
#  params[:category] = 'rock/classic'
#  params[:title] = 'stairway-to-heaven'
```

为了能够模式匹配，你需要分配一个名字给它们，如果没有分配，路由是不会自动解析的。

使用模式匹配时，路由里的 :action 和 :controller 应该以 Hash 的形式传递过来比较好。例如：

```ruby
match 'photos/:id' => 'photos#show'
match 'photos/:id', to: 'photos#show'
match 'photos/:id', controller: 'photos', action: 'show'
```

模式匹配，也可以直接指向 Rack application. 因为它实现了 `call` 方法：

```ruby
match 'photos/:id', to: lambda {|hash| [200, {}, ["Coming soon"]] }
match 'photos/:id', to: PhotoRackApp
# Yes, controller actions are just rack endpoints
match 'photos/:id', to: PhotosController.action(:show)
```

通过HTTP请求，容易带来安全隐患，所以你可以使用 HtttpHelpers[rdoc-ref:HttpHelpers]，而不是 `match`

#### mount 挂载一个基于Rack的应用到我们的程序。

```ruby
match '/movies/search', => "movies#search"
# 去掉语法糖，等价
match '/movies/search', => MoviesController.action(:search).call(env)
```

```ruby
# 这是一个 Rack
MoviesController.action(:search).call(env)

# 这也是一个 Rack
Proc.new { |env|
  [
    200,
    {"Content-Type" => 'text/plain'},
    ["Hello, world"]
  ]
}

# 这还是一个 Rack
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

#### root

实现：

```ruby
def root(options = {})
  match '/', { :as => :root, :via => :get }.merge!(options)
end
```

因为 root 实现基于 match，可以使用相同的可选参数。

建议你把 `root` 放在 `config/routes.rb` 的开头部分，因为 Rails 的匹配规则是从上至下生成的，会优先匹配。

#### 其它方法

```
default_url_options & default_url_options=

has_named_route?

with_default_scope
```
