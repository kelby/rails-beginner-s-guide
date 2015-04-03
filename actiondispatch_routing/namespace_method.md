### namespace

```ruby
namespace :admin do
  resources :posts
end
```

默认对应：

```
    # 中间层 helper 方法  # 最外层网址                    # 最里层 Controller#action
    admin_posts GET       /admin/posts(.:format)          admin/posts#index
    admin_posts POST      /admin/posts(.:format)          admin/posts#create
 new_admin_post GET       /admin/posts/new(.:format)      admin/posts#new
edit_admin_post GET       /admin/posts/:id/edit(.:format) admin/posts#edit
     admin_post GET       /admin/posts/:id(.:format)      admin/posts#show
     admin_post PATCH/PUT /admin/posts/:id(.:format)      admin/posts#update
     admin_post DELETE    /admin/posts/:id(.:format)      admin/posts#destroy
```

1) 使用 `:path` 更改最外层网址：

```
# 使用 /sekret/posts 而不是 /admin/posts
namespace :admin, path: "sekret" do
  resources :posts
end
```

2) 使用 `:module` 更改最里层 Controller#action：

```
# 使用 Sekret::PostsController 而不是 Admin::PostsController
namespace :admin, module: "sekret" do
  resources :posts
end
```

3) 使用 `:as` 更改中间层 helper 方法：

```
# 使用 sekret_posts_path 而不是 admin_posts_path
namespace :admin, as: "sekret" do
  resources :posts
end
```

4) 此外：

```ruby
namespace "admin" do
  resources :kks
end

# 等价于

scope module: "admin", path: "/admin", as: "admin" do
  resources :kks
end
```
