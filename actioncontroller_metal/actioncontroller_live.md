## Live

**实时推送消息。**

尽量不要和 Streaming 搅在一起！有类似之处，但还是请把它们区分开，以便理解！

### 使用 SSE 

SSE 全称 [Server Sent Event](http://www.html5rocks.com/en/tutorials/eventsource/basics/)

提供方法：

```
close

write
```

使用举例：

```ruby
class MyController < ActionController::Base
  # 步骤 1
  include ActionController::Live

  def index
    # 步骤 2
    response.headers['Content-Type'] = 'text/event-stream'

    # 步骤 3 封装了 response.stream
    sse = SSE.new(response.stream, retry: 300, event: "event-name")
    
    # 步骤 4
    sse.write({ name: 'John'})
    sse.write({ name: 'John'}, id: 10)
    sse.write({ name: 'John'}, id: 10, event: "other-event")
    sse.write({ name: 'John'}, id: 10, event: "other-event", retry: 500)
  ensure
    # 步骤 5
    sse.close
  end
end
```

### 不使用 SSE

一般使用都不明确指定 SSE，而是使用默认的 Buffer，举例：

```ruby
class MyController < ActionController::Base
  # 步骤 1
  include ActionController::Live

  def stream
    # 步骤 2
    response.headers['Content-Type'] = 'text/event-stream'

    100.times {
      # 步骤 3 直接使用 response.stream
      response.stream.write "hello world\n"
      sleep 1
    }
  ensure
    # 步骤 4
    response.stream.close
  end
end
```

建议 web server 使用 gem 'puma'，开发环境下默认使用的 WEBrick 不支持。Unicorn(响应比较快，并且对超时比较严格)、Rainbows! 或 Thin 也可以。

### 实例方法

```
process

set_response!

response_body=

log_error
```

`process` 常见的同名方法，这里主要是另开一线程进行处理请求。

`set_response!` 也是常见的同名方法，这里主要是设置 response 为 Live::Response 的实例对象。

`response_body=` 主要是用于确保 response 用完后关闭。

`log_error` 记录错误(有的话)。

Live 可用于构建实时聊天之类等。

参考

[#401 ActionController::Live](http://railscasts.com/episodes/401-actioncontroller-live)
<br>
[Is it live?](http://tenderlovemaking.com/2012/07/30/is-it-live.html)
