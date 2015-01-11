### Resources

常用方法：

| 方法 | 解释 |
| -- | -- |
| resource | 我们的资源不需要集合操作的时候可以使用 |
| resources | 我们的资源需要集合操作的时候可以使用，用得最多 |
| match | 生成路由规则，并添加到 @set 里 |

#### resource

单个资源。

调用了 collection 和 new，以及 set_member_mappings_for_resource 完成所有。

```
      resource
         |
         V
collection 和 new 和 member
         |
         V
       scope
```

在这里：colleciton 完成 create；new 完成 new；member 完成 edit、show、update 和 destroy.

#### resources

多个资源。

调用了 collection 和 new，以及 set_member_mappings_for_resource 完成所有。

```
     resources
         |
         V
collection 和 new 和 member
         |
         V
       scope
```

在这里：colleciton 完成 index 和 create；new 完成 new；member 完成 edit、show、update 和 destroy.

#### match

匹配 url 到一个或多个路由。所有符号，都会对应着 url 里的参数，可用 `params` 获取：

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

# YourController.action(:your_action) 也是 rack endpoint
match 'photos/:id', to: PhotosController.action(:show)
```

通过 HTTP 请求，容易带来安全隐患，所以你可以使用 HtttpHelpers[rdoc-ref:HttpHelpers]，而不是 `match`

另：

```
   match
     |
     V
decomposed_match (还分几种情况)
     |
     V
  add_route
     |
     V
@set.add_route
```

#### 其它：

其它方法：

```
nested 和 root
       |
       V
     scope
```

```
namespace
    |
    V
  super (即 Scoping 里的 namespace)
    |
    V
  scope
```

```
resources_path_names

shallow
shallow?

using_match_shorthand?
```

protected 方法：

```
set_member_mappings_for_resource

with_exclusive_scope

with_scope_level
```

`set_member_mappings_for_resource` 我们路由里的 edit、show、update 和 destroy 由它完成。

还有：

```ruby
VALID_ON_OPTIONS = [:new, :collection, :member]
RESOURCE_OPTIONS = [:as, :controller, :path, :only, :except, :param, :concerns]
```
