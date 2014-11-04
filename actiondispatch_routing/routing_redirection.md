## Redirection

`redirect(*args, &block)`

在路由里配置重定向，将发向某路径的请求，重定向到另一路径：

```ruby
get "/stories" => redirect("/posts")
```

你甚至可以在重定向的参数里使用 %{} 求值：

```ruby
get 'docs/:article', to: redirect('/wiki/%{article}')
```

在路由系统里就完成重定向了，不经过 ActionController 等处理。在浏览器里可以看到网址的跳转。
