### scope

接受参数：

path、constraints、shallow_path、shallow_prefix、defaults、as、module、controller

**defaults 参数**

它是通用的，类型为 Hash.

**as 参数** 3 个很重要的参数之一

```ruby
# 只影响中间 helper 方法
scope as: "sekret" do
  resources :posts
end
```

**path 参数** 3 个很重要的参数之一

```ruby
# 只影响最外面的网址
scope path: "/admin" do
  resources :posts
end
```

**module 参数** 3 个很重要的参数之一

```ruby
# 只影响最里面的 Controller#action
scope module: "admin" do
  resources :posts
end
```

**path 和 as 参数**

```ruby
# 影响中间 helper 方法 和 影响最外面的网址
scope path: ":account_id", as: "account" do
  resources :projects
end
```

**constraints 参数**

对应着 constraints 方法。

**controller 参数**

对应着 controller 方法。


