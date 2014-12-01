## Metal 之外

## Cookies

提供了 `cookies` 方法，本质是 request.cookie_jar

## Etag With Template Digest

为了配合生成 ETag，默认 ActionView 静态内容，已经加密(区别于片段缓存里对静态内容的加密). 可以设置取消：

```ruby
config.action_controller.etag_with_template_digest = false
```

fresh_when 匹配的只是动态资源。静态资源怎么办？

静态资源经过加密，每一次更改，Hash 值都会改变，所以重启服务器后都会刷新。
取消后，静态资源不经过加密，更改它们，Hash 值并不会更改，所以重启服务器得到的还是原来的内容。

本设置仅针对 ETag，若在其它浏览器访问网站，得到的仍然是正确内容。

## Flash

提供 add_flash_types 方法，并对 redirect_to 稍微做了处理。在 Others#Flash 章节会有专门介绍。

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
action_methods  # 覆盖 AbstractController::Base#action_methods，结果并不准确
hide_action     # 将 action 声明为 hide
visible_action? # 判断 action 是否 hide?
```

其中，使用 `hide_action` 后 public 方法，不再被当做 action.

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

send_data
send_file
```

## ~~Rack Delegation~~

```
dispatch

reset_session

response_body=
```

很多模块都包含它，利用了它所提供的方法。

## ~~Rendering~~

对一般的 render_to_body 和 render_to_string 稍微做处理。

## ~~Rescue~~

配置是否 show_detailed_exceptions?

如果是的话，有异常时除了执行 ActiveSupport 的 rescue_with_handler 外，还额外执行本模块提供的 rescue_with_handler.

## ~~Url For~~

提供 url_options 方法，不会直接使用, 但会影响到 `url_for` 的结果。
