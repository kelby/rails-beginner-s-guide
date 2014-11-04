## Metal 之外

## Cookies

提供了 `cookies` 方法，本质是 request.cookie_jar

## Data Streaming

```
send_data # 封装了方法 render
send_file # 封装了内置库 File
```

发送二进制数据或文件给浏览器，功能类似 render.

实际项目里，静态资源可以通过 Web服务器(Nginx/Apache)发送，应用只要提供 URL 即可。

## Etag With Template Digest

默认 ActionView 已经使用 ETag. 可以设置取消：

```ruby
config.action_controller.etag_with_template_digest = false
```

## Flash

对一般的 redirect_to 稍微做处理。在 Others#Flash 章节会有专门介绍。

## Force SSL

```
force_ssl_redirect
```

重定向到 https 链接, url 会改变。

如果没有(用参数)指定链接，则用当前链接，但是 https 协议访问。可用于注册、登录等页面。

上面是实例方法，对单个 action 有用；下面这是类方法，对整个 Controller 有用。

```
force_ssl
```

使用举例：

```ruby
class AccountsController < ApplicationController
  force_ssl if: :ssl_configured?

  def ssl_configured?
    !Rails.env.development?
  end
end
```

## Helpers

```
helpers

all_helpers_from_path
helper_attr
modules_for_helpers
```

其中 

```ruby
ApplicationController.helpers.class
# => ActionView::Base
```

控制台里的 helper 就是 ActionView::Base 的实例对象。

## Hide Actions

```
action_methods  # 覆盖 AbstractController::Base#action_methods
hide_action     # 将 action 声明为 hide
visible_action? # 判断 action 是否 hide?
```

## ~~Implicit Render~~

默认渲染，它的优先级很高。

```
# AbstractController::Base 有同名方法，根据 Ruby 调用规则，这里会被优先调用
send_action

default_render
method_for_action
```

## ~~Instrumentation~~

当执行以下同名方法时，会有时间记录(以观察性能等指标)。

```
process_action
redirect_to
render
send_data, send_file
```

## ~~Rack Delegation~~

```
dispatch
reset_session
response_body=
```

很多模块都包含它，利用了它所提供的方法。

## Redirecting

```
redirect_to
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

## Rendering

对一般的 render_to_body 和 render_to_string 稍微做处理。

## ~~Rescue~~

配置是否 show_detailed_exceptions?

如果是的话，有异常时除了执行一般的 rescue_with_handler 外，还额外执行本模块提供的 rescue_with_handler.

## ~~UrlFor~~

提供 url_options 方法，不会直接使用, 但会影响到 `url_for` 的结果。
