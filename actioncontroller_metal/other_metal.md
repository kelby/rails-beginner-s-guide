## 其它

### Cookies

提供了 `cookies` 方法，本质是 request.cookie_jar

### ~~Implicit Render~~

默认渲染，它的优先级很高。

```
send_action

default_render

method_for_action
```

假设，我们在 Controller#action 里，没有 render 模板或返回数据 ... 它们就有用了。

### ~~Instrumentation~~

当执行以下同名方法时，会发送消息，及有时间记录(以观察性能等指标)。

```
process_action

redirect_to

render

send_data
send_file
```

用到了 ActiveSupport::Notifications.instrument 和 Benchmark.ms

### ~~Rack Delegation~~

```
dispatch

reset_session

response_body=
```

`set_response!` 定义 response 为 ActionDispatch::Response 实例对象，并初始化。

`reset_session` 重置 session.

很多模块都包含了同名方法，有的是功能上的简单封装，有的是重写。

另外，还有：

```
delegate :headers, :status=, :location=, :content_type=,
         :status, :location, :content_type, :response_code, :to => "@_response"
```

### ~~Rendering~~

对一般的 render_to_body 和 render_to_string 稍微做处理。

### ~~Rescue~~

执行到具体 action 抛异常时会检测是否需要抛异常，如果是的话，抛异常。

有 process_action 同名方法。

show_detailed_exceptions? (默认是 false，但本地请求的话是 true)，可配置 config.consider_all_requests_local

rescue_with_handler(封装了 ActiveSupport::Rescuable 的同名方法)

### ~~Url For~~

`url_for` 方法的组成部分。
