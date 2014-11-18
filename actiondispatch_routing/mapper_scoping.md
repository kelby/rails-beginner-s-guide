### Scoping

有选择的讲：

| 方法 | 解释 |
| -- | -- |
| scope | 当几个路由规则的可选参数(部分)都一样的时候，可以用`scope`把它们包住，避免重复 |
| namespace | 基于 scope，只是设置了 :module => path <br> 本质是设置了 scope 的 :path、:module 和 :as 参数 |
| controller | 基于 scope，只是设置了 :controller => controller <br>和普通写法差不多，只是把单行改为了多行的形式 |
| constraints | 基于 scope，只是设置了 :constraints <br>传递的是限制条件，符合条件的请求才会进入里面的路由，进行下一步操作。 |
| defaults | 基于 scope，只是设置了 :defaults <br>设置默认值|

controller 和 constraints 仅做约束条件，不影响路由的'一个罗卜，一个坑'的特性。其它几个方法，看情况。

**比较 namespace 和 scope**

|            | namespace | scope(第一个参数是字符串)  |
| :--------: | :--------| :--   |
| 同         | 默认在 url 里有前缀   |  默认在 url 里有前缀   |
| 异         |  默认Controller有命名空间 |  默认Controller无命名空间  |

scope 可选参数：
```ruby
SCOPE_OPTIONS = [:path, :shallow_path, :as, :shallow_prefix, :module,
                       :controller, :action, :path_names, :constraints,
                       :shallow, :blocks, :defaults, :options]
```

### 使用举例：

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

---


标准是：

            jjs GET    /jjs(.:format)                      jjs#index
                POST   /jjs(.:format)                      jjs#create
         new_jj GET    /jjs/new(.:format)                  jjs#new
        edit_jj GET    /jjs/:id/edit(.:format)             jjs#edit
             jj GET    /jjs/:id(.:format)                  jjs#show
                PATCH  /jjs/:id(.:format)                  jjs#update
                PUT    /jjs/:id(.:format)                  jjs#update
                DELETE /jjs/:id(.:format)                  jjs#destroy
                
scope "/admin" 是：影响最外层(**也就是说默认是 :path**)

            iis GET    /admin/iis(.:format)                iis#index
                POST   /admin/iis(.:format)                iis#create
         new_ii GET    /admin/iis/new(.:format)            iis#new
        edit_ii GET    /admin/iis/:id/edit(.:format)       iis#edit
             ii GET    /admin/iis/:id(.:format)            iis#show
                PATCH  /admin/iis/:id(.:format)            iis#update
                PUT    /admin/iis/:id(.:format)            iis#update
                DELETE /admin/iis/:id(.:format)            iis#destroy

scope module: "admin" 是 影响最里层

            hhs GET    /hhs(.:format)                      admin/hhs#index
                POST   /hhs(.:format)                      admin/hhs#create
         new_hh GET    /hhs/new(.:format)                  admin/hhs#new
        edit_hh GET    /hhs/:id/edit(.:format)             admin/hhs#edit
             hh GET    /hhs/:id(.:format)                  admin/hhs#show
                PATCH  /hhs/:id(.:format)                  admin/hhs#update
                PUT    /hhs/:id(.:format)                  admin/hhs#update
                DELETE /hhs/:id(.:format)                  admin/hhs#destroy

scope path: "/admin" 是 影响最外层

            ggs GET    /admin/ggs(.:format)                ggs#index
                POST   /admin/ggs(.:format)                ggs#create
         new_gg GET    /admin/ggs/new(.:format)            ggs#new
        edit_gg GET    /admin/ggs/:id/edit(.:format)       ggs#edit
             gg GET    /admin/ggs/:id(.:format)            ggs#show
                PATCH  /admin/ggs/:id(.:format)            ggs#update
                PUT    /admin/ggs/:id(.:format)            ggs#update
                DELETE /admin/ggs/:id(.:format)            ggs#destroy

scope as: "admin" 是 影响中间层

        admin_ffs GET    /ffs(.:format)                      ffs#index
                  POST   /ffs(.:format)                      ffs#create
     new_admin_ff GET    /ffs/new(.:format)                  ffs#new
    edit_admin_ff GET    /ffs/:id/edit(.:format)             ffs#edit
         admin_ff GET    /ffs/:id(.:format)                  ffs#show
                  PATCH  /ffs/:id(.:format)                  ffs#update
                  PUT    /ffs/:id(.:format)                  ffs#update
                  DELETE /ffs/:id(.:format)                  ffs#destroy


namespace :admin 影响 3 者


        admin_dds GET    /admin/dds(.:format)                admin/dds#index
                  POST   /admin/dds(.:format)                admin/dds#create
     new_admin_dd GET    /admin/dds/new(.:format)            admin/dds#new
    edit_admin_dd GET    /admin/dds/:id/edit(.:format)       admin/dds#edit
         admin_dd GET    /admin/dds/:id(.:format)            admin/dds#show
                  PATCH  /admin/dds/:id(.:format)            admin/dds#update
                  PUT    /admin/dds/:id(.:format)            admin/dds#update
                  DELETE /admin/dds/:id(.:format)            admin/dds#destroy


