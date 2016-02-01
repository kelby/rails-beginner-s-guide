### scope

#### 一、不使用 scope

```ruby
resources :jjs
```

默认对应：

            jjs GET    /jjs(.:format)                      jjs#index
                POST   /jjs(.:format)                      jjs#create
         new_jj GET    /jjs/new(.:format)                  jjs#new
        edit_jj GET    /jjs/:id/edit(.:format)             jjs#edit
             jj GET    /jjs/:id(.:format)                  jjs#show
                PATCH  /jjs/:id(.:format)                  jjs#update
                PUT    /jjs/:id(.:format)                  jjs#update
                DELETE /jjs/:id(.:format)                  jjs#destroy

#### 二、使用 scope，但不传递参数

```ruby
scope "/admin" do
  resources :iis
end
```

默认对应：

            iis GET    /admin/iis(.:format)                iis#index
                POST   /admin/iis(.:format)                iis#create
         new_ii GET    /admin/iis/new(.:format)            iis#new
        edit_ii GET    /admin/iis/:id/edit(.:format)       iis#edit
             ii GET    /admin/iis/:id(.:format)            iis#show
                PATCH  /admin/iis/:id(.:format)            iis#update
                PUT    /admin/iis/:id(.:format)            iis#update
                DELETE /admin/iis/:id(.:format)            iis#destroy

可以看出，只影响最外面的网址。

#### 三、使用 scope，并传递参数

接受参数：path、constraints、shallow_path、shallow_prefix、defaults、as、module、controller 等。

1) **:as 参数**

3 个很重要的参数之一

```ruby
# 只影响中间 helper 方法
scope as: "admin" do
  resources :ffs
end
```

影响中间层

        admin_ffs GET    /ffs(.:format)                      ffs#index
                  POST   /ffs(.:format)                      ffs#create
     new_admin_ff GET    /ffs/new(.:format)                  ffs#new
    edit_admin_ff GET    /ffs/:id/edit(.:format)             ffs#edit
         admin_ff GET    /ffs/:id(.:format)                  ffs#show
                  PATCH  /ffs/:id(.:format)                  ffs#update
                  PUT    /ffs/:id(.:format)                  ffs#update
                  DELETE /ffs/:id(.:format)                  ffs#destroy

2) **:path 参数**

3 个很重要的参数之一

```ruby
# 只影响最外面的网址
scope path: "/admin" do
  resources :ggs
end
```

影响最外层

            ggs GET    /admin/ggs(.:format)                ggs#index
                POST   /admin/ggs(.:format)                ggs#create
         new_gg GET    /admin/ggs/new(.:format)            ggs#new
        edit_gg GET    /admin/ggs/:id/edit(.:format)       ggs#edit
             gg GET    /admin/ggs/:id(.:format)            ggs#show
                PATCH  /admin/ggs/:id(.:format)            ggs#update
                PUT    /admin/ggs/:id(.:format)            ggs#update
                DELETE /admin/ggs/:id(.:format)            ggs#destroy

3) **:module 参数**

3 个很重要的参数之一

```ruby
# 只影响最里面的 Controller#action
scope module: "admin" do
  resources :hhs
end
```

影响最里层

            hhs GET    /hhs(.:format)                      admin/hhs#index
                POST   /hhs(.:format)                      admin/hhs#create
         new_hh GET    /hhs/new(.:format)                  admin/hhs#new
        edit_hh GET    /hhs/:id/edit(.:format)             admin/hhs#edit
             hh GET    /hhs/:id(.:format)                  admin/hhs#show
                PATCH  /hhs/:id(.:format)                  admin/hhs#update
                PUT    /hhs/:id(.:format)                  admin/hhs#update
                DELETE /hhs/:id(.:format)                  admin/hhs#destroy

4) **:path、:as 和 :module 参数**

```ruby
scope path: ":admin", as: "admin", module: 'admin' do
  resources :dds
end
```

等价于

namespace :admin

        admin_dds GET    /admin/dds(.:format)                admin/dds#index
                  POST   /admin/dds(.:format)                admin/dds#create
     new_admin_dd GET    /admin/dds/new(.:format)            admin/dds#new
    edit_admin_dd GET    /admin/dds/:id/edit(.:format)       admin/dds#edit
         admin_dd GET    /admin/dds/:id(.:format)            admin/dds#show
                  PATCH  /admin/dds/:id(.:format)            admin/dds#update
                  PUT    /admin/dds/:id(.:format)            admin/dds#update
                  DELETE /admin/dds/:id(.:format)            admin/dds#destroy

5) **:defaults 参数**

对应着 defaults 方法。(它是通用的，类型为 Hash.)

6) **:constraints 参数**

对应着 constraints 方法。

7) **:controller 参数**

对应着 controller 方法。
