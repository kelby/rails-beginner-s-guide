## Live & Streaming

### Live

```
log_error
process

response_body=

set_response!
```

### 使用 SSE (Server Sent Event)

提供方法：

```
close
write
```

使用举例：

```ruby
class MyController < ActionController::Base
  include ActionController::Live

  def index
    response.headers['Content-Type'] = 'text/event-stream'
    sse = SSE.new(response.stream, retry: 300, event: "event-name")
    sse.write({ name: 'John'})
    sse.write({ name: 'John'}, id: 10)
    sse.write({ name: 'John'}, id: 10, event: "other-event")
    sse.write({ name: 'John'}, id: 10, event: "other-event", retry: 500)
  ensure
    sse.close
  end
end
```

### 不使用 SSE

一般使用都不明确指定 SSE，而是使用默认的 Buffer，举例：

```ruby
class MyController < ActionController::Base
  include ActionController::Live

  def stream
    response.headers['Content-Type'] = 'text/event-stream'
    100.times {
      response.stream.write "hello world\n"
      sleep 1
    }
  ensure
    response.stream.close
  end
end
```

### Streaming

Streaming 没有对外提供方法，但使用 `render` 的时候, 它对应着 :stream 参数

```ruby
class PostsController
  def index
    @posts = Post.all
    render stream: true
  end
end
```

**Streaming & Live**

Rails 默认的渲染过程先是模板，然后才是布局。完成模板，及需要的查询，最后才完成布局。

Streaming 可以改变一下顺序，按布局来渲染。布局先显示，对于用户体验可能更好一点，还有就是这会使得JS和CSS的加载顺序比平时提前。

使用很简单，在 `render` 的时候，加上 `:stream` 参数即可：

```ruby
class PostsController
  def index
    @posts = Post.all
    render stream: true
  end
end
```

Live 用于构建实时聊天之类的。
