## Redirection

**路由里的 redirect 方法。**

`redirect(*args, &block)`

在 routes.rb 里配置重定向，将发向某路径的请求，重定向到另一路径：

```ruby
get "/stories" => redirect("/posts")
```

你也可以动态的重定向到新的路径：

```ruby
get 'docs/:article', to: redirect('/wiki/%{article}')
```

这里的重定向，在路由里完成，不经过 Action Controller 等处理。
<br>
并且，在浏览器里会看到 url 的改变。
