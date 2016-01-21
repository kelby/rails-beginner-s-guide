## Redirecting

```
redirect_to

redirect_back
```

重要的部分就是可以根据不同的可选参数，计算出要重定向的 url.

使用举例：

```ruby
# 字符串
redirect_to “www.rubyonrails.org”
redirect_to “/images/screenshot.jpg”
redirect_to articles_url

# :back
redirect_to :back

# Proc
redirect_to proc { edit_post_url(@post) }

# 其它可选参数，会用到 url_for 来处理

# record 对象
redirect_to post

# Hash
redirect_to action: “show”, id: 5
redirect_to({ action: 'atom' }, alert: "Something serious happened")
```

相关、类似功能：

`url_for` 根据给定的参数和 default_url_options 和 routes.rb 里的路由定义，生成可用的 url.

`polymorphic_url` 根据传递的 record 对象，构建可用的 url.

极端情况下，才会发生：

```
 redirect_to
     |
     V
ActionController::UrlFor
     |
     V
  url_for
     |
     V
AbstractController::UrlFor
     |
     V
ActionDispatch::Routing::UrlFor
     |
     V
ActionDispatch::Routing::PolymorphicRoutes
     |
     V
polymorphic_url
     |
     V
  ... ...
```

`redirect_back` 相当于来的 `redirect_to :back` 但它可接受 `:fallback_location` 参数，以应对 Referer 不存在的问题。
