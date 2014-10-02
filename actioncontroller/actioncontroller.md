Controller 里的 public方法(也就是action) 会自动对应 Route 里的路由规则。当请求到来时，action 接受请求并处理，最后渲染相应视图模板(Get-and-show)或重定向到另一 action(do-and-redirect)。

默认，只有 ApplicationController 直接继承于 ActionController::Base，其它的控制器继承于 ApplicationController。所以，如果你想在所有 controller 处理之前做一些什么，你可以把它们写在 ApplicationController 里。

## Metal

**ActionController::Metal** 本身是一个功能极简的 Controller, 但它符合 Rack 接口规范，所以也可以把它称之为 Rack application. 相比于 **ActionController::Base** 它的功能真的很有限。

如你的 Rails 项目对性能要求比较高，或者说对实时性要求比较高，又或者你的项目做为 API 对外提供服务，那么你可以尝试直接继承使用 Metal.

> Rails Metal is a subset of Rack middleware


## Metal 之外

Metal 仅包含 metal.rb 这个文件，不包含其同名目录。下面对 metal/ 目录下面包含的东西，做一下简述：

### Redirecting

```ruby
redirect_to(options = {}, response_status = {})
```

是重要的部分就是根据不同的 options，计算出 url 路径：

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

# - Record
redirect_to post

# - Hash
redirect_to({ action: 'atom' }, alert: "Something serious happened")
redirect_to action: “show”, id: 5
```

相关、类似功能：

`url_for` Generate a url based on the options provided, default_url_options and the routes defined in routes.rb.

`polymorphic_url` Constructs a call to a named RESTful route for the given record and returns the resulting URL string。

极端情况下，会发生 redirect_to -> url_for -> ActionController::UrlFor -> AbstractController::UrlFor -> ActionDispatch::Routing::UrlFor --> ActionDispatch::Routing::PolymorphicRoutes -> polymorphic_url

## 其它

### Http Authentication

有3类：Basic，Digest，Token

我们只看一个最最常用的：

```ruby
authenticate_or_request_with_http_basic(realm = "Application", &login_procedure)
```

## 其它

ActionController include 了这些模块，而我们自定义的 Controller 又继承于 ActionController.

自然的，它们的 ClassMethods 就会变成我们自定义 Controller 的类方法，而其它方法则类似实例方法，可运用于 action.

## 参考

[Introducing Rails Metal](http://weblog.rubyonrails.org/2008/12/17/introducing-rails-metal/)<br>
[Rails on Rack](http://edgeguides.rubyonrails.org/rails_on_rack.html)<br>
[Developing api with rails metal](http://www.slideshare.net/artellectual/developing-api-with-rails-metal)<br>
[Rails Metal](http://railscasts.com/episodes/150-rails-metal)<br>
[基于资源的HTTP Cache的实现介绍](http://robbinfan.com/blog/13/http-cache-implement)
