# Metal 其它

## Cookies

提供了 `cookies` 方法，本质是 request.cookie_jar

## Flash

提供 `add_flash_types` 方法，并对 redirect_to 稍微做了处理。在【其它#Flash 的使用】章节会有专门介绍。

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

如果是的话，有异常时除了执行 Active Support 的 rescue_with_handler 外，还额外执行本模块提供的 rescue_with_handler.

## ~~Url For~~

提供 url_options 方法，不会直接使用, 但会影响到 `url_for` 的结果。
