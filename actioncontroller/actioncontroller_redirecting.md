## Redirecting

```
redirect_to(options = {}, response_status = {})
```

也就是重定向，运行多种参数，使用举例：

```
redirect_to action: "show", id: 5
redirect_to post
redirect_to "http://www.rubyonrails.org"
redirect_to "/images/screenshot.jpg"
redirect_to articles_url
redirect_to :back
redirect_to proc { edit_post_url(@post) }
```

根据参数情况，有可能会用到 ActionDispatch 提供的 url_for.


重要的部分就是可以根据不同的 options，计算出 url 路径：

```ruby
# 计算出 url 路径
def _compute_redirect_to_location(options) #:nodoc:
  case options
  # The scheme name consist of a letter followed by any combination of
  # letters, digits, and the plus ("+"), period ("."), or hyphen ("-")
  # characters; and is terminated by a colon (":").
  # See http://tools.ietf.org/html/rfc3986#section-3.1
  # The protocol relative scheme starts with a double slash "//".
  when /\A([a-z][a-z\d\-+\.]*:|\/\/).*/i
    options
  when String
    request.protocol + request.host_with_port + options
  when :back
    request.headers["Referer"] or raise RedirectBackError
  when Proc
    _compute_redirect_to_location options.call
  else
    url_for(options)
  end.delete("\0\r\n")
end
```

根据不同的 options，计算出 url 路径，使用举例：

```ruby
# String
redirect_to “www.rubyonrails.org”
redirect_to “/images/screenshot.jpg”
redirect_to articles_url

# :back
redirect_to :back

# Proc
redirect_to proc { edit_post_url(@post) }

# 其它举例(由 url_for 处理)：

# record 对象
redirect_to post

# Hash
redirect_to({ action: 'atom' }, alert: "Something serious happened")
redirect_to action: “show”, id: 5
```

相关、类似功能：

`url_for` 根据给定的参数和 default_url_options 和 routes.rb 里的路由定义这 3 者，生成可用的 url.

`polymorphic_url` 根据传递的 record 对象，构建可用的 url.

极端情况下，会发生 redirect_to -> url_for -> ActionController::UrlFor -> AbstractController::UrlFor -> ActionDispatch::Routing::UrlFor --> ActionDispatch::Routing::PolymorphicRoutes -> polymorphic_url
