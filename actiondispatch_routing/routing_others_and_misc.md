# Routing others & misc

复数 + 具名：

```ruby
resources :photos

resources :magazines do
  resources :ads
end
```

单数 + 具名

```
resource :profile
```

不具名

```
所见即所得
```

如何嵌套？

嵌套不具名

嵌套资源类似。
