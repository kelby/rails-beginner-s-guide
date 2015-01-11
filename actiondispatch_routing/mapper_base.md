### Base

常用方法：

```
match

mount

root
```

#### match

这里的 match 只是个同名方法，是个空壳子，详细要看 Resources 里的。

另外，mount 和 root 本质上，都是封装和扩展 `match` 方法。

```
mount 和 root
      |
      V
    match
```

#### mount - 挂载一个基于 Rack 的应用到我们的程序。

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
