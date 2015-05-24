### Scoping

#### scope

当几个路由规则的可选参数(部分)都一样的时候，可以用`scope`把它们包住，避免重复。

`scope` 并不会添加路由，但它会设置 @scope 的值，从而影响如何生成路由。

```ruby
def scope(*args)
  # ... ...

  @scope.options.each do |option|
    # ...
  end

  @scope = @scope.new scope
  yield
  self
ensure
  @scope = @scope.parent
end
```

另，`@scope` 是 Scope 的实例对象。

#### 以下几个方法，直接封装于它：

| 方法 | 解释 |
| -- | -- |
| namespace | 基于 scope，只是设置了 :module => path <br> 本质是设置了 scope 的 :path、:module 和 :as 参数 |
| controller | 基于 scope，只是设置了 :controller => controller <br>和普通写法差不多，只是把单行改为了多行的形式 |
| constraints | 基于 scope，只是设置了 :constraints <br>传递的是限制条件，符合条件的请求才会进入里面的路由，进行下一步操作。 |
| defaults | 基于 scope，只是设置了 :defaults <br>设置默认值|

`scope` 可选参数：
```ruby
SCOPE_OPTIONS = [:path, :shallow_path, :as, :shallow_prefix, :module,
                       :controller, :action, :path_names, :constraints,
                       :shallow, :blocks, :defaults, :options]
```

`controller` 和 `constraints` 仅做约束条件，不影响路由的'一个罗卜，一个坑'的特性。其它几个方法，看情况。

#### 使用举例：

```ruby
scope module: "admin" do
  resources :posts
end

# 或

resources :posts, module: "admin"
```

可将 "/posts" 路由到 Admin::PostsController

```ruby
scope "/admin" do
  resources :posts
end

# 或

resources :posts, path: "/admin/posts"
```

可将 "/admin/posts" 路由到 PostsController
