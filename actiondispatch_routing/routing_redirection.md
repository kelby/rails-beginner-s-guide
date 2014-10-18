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
