### namespace

```ruby
namespace :admin do
  resources :posts
end
```

```
    # 中间 helper 方法     # 最外面网址                     # 最里面 Controller#action
    admin_posts GET       /admin/posts(.:format)          admin/posts#index
    admin_posts POST      /admin/posts(.:format)          admin/posts#create
 new_admin_post GET       /admin/posts/new(.:format)      admin/posts#new
edit_admin_post GET       /admin/posts/:id/edit(.:format) admin/posts#edit
     admin_post GET       /admin/posts/:id(.:format)      admin/posts#show
     admin_post PATCH/PUT /admin/posts/:id(.:format)      admin/posts#update
     admin_post DELETE    /admin/posts/:id(.:format)      admin/posts#destroy
```

使用 :path 更改最外面网址

```
# accessible through /sekret/posts rather than /admin/posts
namespace :admin, path: "sekret" do
  resources :posts
end
```

使用 :module 更改最里面 Controller#action

```
# maps to <tt>Sekret::PostsController</tt> rather than <tt>Admin::PostsController</tt>
namespace :admin, module: "sekret" do
  resources :posts
end
```

使用 :as 更改中间 helper 方法

```
# generates +sekret_posts_path+ rather than +admin_posts_path+
namespace :admin, as: "sekret" do
  resources :posts
end
```

---

相等性

```ruby
namespace "admin" do
  resources :kks
end

# 等价于

scope module: "admin", path: "/admin", as: "sekret" do
  resources :lls
end
```
