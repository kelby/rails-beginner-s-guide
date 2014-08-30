# ActionDispatch Routing

ActionDispatch 本身很复杂，但我们使用 Rails 进行开发的过程中接触到的主要就是 Mapper 这部分。

也就是路由机制这部分，它包括：Base、Concerns、HttpHelpers、Resources、Scoping

## Mapper

### Base

- `match(path, options=nil)`

匹配url到一个或多个路由。所有符号，都会对应着url里的参数，可用`params`获取：

```ruby
# sets :controller, :action and :id in params
match ':controller/:action/:id'
```

预留了两个符号，`:controller` 对应着Controller，`:action` 对应着Action。也可以接受模式匹配做为参数：

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

A pattern can also point to a Rack endpoint i.e. anything that responds to call:
模式匹配，也可以直接指向 Rack。只要它实现了 `call` 方法：

```ruby
match 'photos/:id', to: lambda {|hash| [200, {}, ["Coming soon"]] }
match 'photos/:id', to: PhotoRackApp
# Yes, controller actions are just rack endpoints
match 'photos/:id', to: PhotosController.action(:show)
```

通过HTTP请求，容易带来安全隐患，所以你可以使用 HtttpHelpers[rdoc-ref:HttpHelpers]，而不是 `match`

- `mount(app, options = nil)` 挂载一个基于Rack的应用到我们的程序。

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

- `root(options = {})`

实现：

```ruby
def root(options = {})
  match '/', { :as => :root, :via => :get }.merge!(options)
end
```

因为 root 实现基于 match，可以使用相同的可选参数。

建议你把 `root` 放在 `config/routes.rb` 的开头部分，因为 Rails 的匹配规则是从上至下生成的，会优先匹配。

### Concerns

为了防止 `config/routes.rb` 里的重复代码而来的。

`concern(name, callable = nil, &block)` 定义(一次只能定义一个)
`concerns(*args)` 调用(一次可调用多个)

```ruby
concern :commentable do
  resources :comments
end

concern :image_attachable do
  resources :images, only: :index
end

# 配合 resources
resources :messages, concerns: [:commentable, :image_attachable]

# 配合 scope 或 namespace
namespace :posts do
  concerns :commentable
end
```

### HttpHelpers

对应 HTTP 请求里的：

```ruby
delete(*args, &block)
get(*args, &block)
patch(*args, &block)
post(*args, &block)
put(*args, &block)
```

只是语法糖，都是封装了 `match`

```ruby
# 第一个参数 method 表示：delete, get, patch, post, put
def map_method(method, args, &block)
  options = args.extract_options!
  options[:via] = method
  match(*args, options, &block)
  self
end
```

### Resources

有选择的讲：

```ruby
collection() - 嵌套于 resources
member() - 嵌套于 resources
resource(*resources, &block) - 对应的是单数
resources(*resources, &block) - 对应的是复数
```

```
match - 同名
namespace - 同名
root - 同名
```

VALID_ON_OPTIONS  = [:new, :collection, :member]
RESOURCE_OPTIONS  = [:as, :controller, :path, :only, :except, :param, :concerns]

### Scoping

有选择的讲：
```ruby
namespace(path, options = {}) - 基于 scope，只是设置了 :module => path
controller(controller, options={}) - 基于 scope，只是设置了 :controller => controller
constraints(constraints = {}) - 基于 scope，只是设置了 :constraints
defaults(defaults = {}) - 基于 scope，只是设置了 :defaults

scope(*args) - 当几个路由规则的可选参数(部分)都一样的时候，可以用`scope`把它们包住，避免重复
```

|            | namespace | scope(第一个参数是字符串)  |
| :--------: | :--------:| :--:   |
| 同         | 默认在 url 里有前缀   |  默认在 url 里有前缀   |
| 异         |  默认Controller有命名空间 |  默认Controller无命名空间  |

scope 可选参数：
```ruby
SCOPE_OPTIONS = [:path, :shallow_path, :as, :shallow_prefix, :module,
                       :controller, :action, :path_names, :constraints,
                       :shallow, :blocks, :defaults, :options]
```

## PolymorphicRoutes
------------

If you want to generate different urls according to different objects(例如，多态：`belongs_to :commentable, :polymorphic => true`), you should use the polymorphic_path/polymorphic_url to simplify the url generation.

`polymorphic_url(record_or_hash_or_array, options = {})`

Constructs a call to a named RESTful route for the given record and returns the resulting URL string. For example:

```ruby
# calls post_url(post)
polymorphic_url(post) # => "http://example.com/posts/1"
polymorphic_url([blog, post]) # => "http://example.com/blogs/1/posts/1"
polymorphic_url([:admin, blog, post]) # => "http://example.com/admin/blogs/1/posts/1"
polymorphic_url([user, :blog, post]) # => "http://example.com/users/1/blog/posts/1"
polymorphic_url(Comment) # => "http://example.com/comments"
```

除了 Mapper 外，还用到的就只剩：Redirection、PolymorphicRoutes、UrlFor

## 其它
-------

### Redirection

`redirect(*args, &block)`

在路由里配置重定向，将发向某路径的请求，重定向到另一路径：

```
get "/stories" => redirect("/posts")
```

你甚至可以在重定向的参数里使用 %{} 求值：

```
get 'docs/:article', to: redirect('/wiki/%{article}')
```
Note that if you return a path without a leading slash then the url is prefixed with the current SCRIPT_NAME environment variable. This is typically '/' but may be different in a mounted engine or where the application is deployed to a subdirectory of a website.

Alternatively you can use one of the other syntaxes:

The block version of redirect allows for the easy encapsulation of any logic associated with the redirect in question. Either the params and request are supplied as arguments, or just params, depending of how many arguments your block accepts. A string is required as a return value.

```ruby
get 'jokes/:number', to: redirect { |params, request|
  path = (params[:number].to_i.even? ? "wheres-the-beef" : "i-love-lamp")
  "http://#{request.host_with_port}/#{path}"
}
```

Note that the +do end+ syntax for the redirect block wouldn't work, as Ruby would pass the block to get instead of redirect. Use { ... } instead.

The options version of redirect allows you to supply only the parts of the url which need to change, it also supports interpolation of the path similar to the first example.

```
get 'stores/:name',       to: redirect(subdomain: 'stores', path: '/%{name}')
get 'stores/:name(*all)', to: redirect(subdomain: 'stores', path: '/%{name}%{all}')
```

Finally, an object which responds to call can be supplied to redirect, allowing you to reuse common redirect routes. The call method must accept two arguments, params and request, and return a string.

```
get 'accounts/:name' => redirect(SubdomainRedirector.new('api'))
```

