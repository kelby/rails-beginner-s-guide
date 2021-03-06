## Channel

可直接调用。类似 Controller ... 外部请求过来，MVC 里最先由它处理。

它处理从客户端过来的 ws 请求。

### Base

完成请求转发，由 Action Dispatch 到具体的 Channel#action. 
<br>
身份地位相当于 Action Controller 里的 Metal + Abstract Controller 里的 Base.

另外，它是 Channel 的头：

```ruby
include Callbacks
include PeriodicTimers
include Streams
include Naming
include Broadcasting
```

**内部处理**

Channel 整体结构比 Controller 简单，这里的 Base 就相当于 Action Controller 里的 Metal.

它有类方法 `action_methods`、实例方法 `perform_action` 和私有方法 `dispatch_action` 等。Action Dispatch 转发请求过来后，主要由它进行处理，转给具体的 Channel#action.

区别也是有的，这边的请求不需要 Middleware 进行处理，所以它没有调用到，而 Metal 需要。

**对外提供接口**

类方法：

```
action_methods
```

```
method_added

clear_action_methods!
```

实例方法：

```ruby
attr_reader :params, :connection, :identifier
delegate :logger, to: :connection
```

```
perform_action

unsubscribe_from_channel
```

`perform_action` public_send 后执行我们自定义的 action.

```ruby
# 实际上由 connection 完成
transmit

# 以下两个由子类实现
subscribed
unsubscribed

# 以下几个相当于配置、问询

reject

defer_subscription_confirmation?
defer_subscription_confirmation!

subscription_confirmation_sent?
subscription_rejected?
```

默认其子类要重写 `subscribed` 和 `unsubscribed` 方法。

`subscribed` 相当于初始化。

如果不允许订阅，可以用 `reject` 进行拒绝。

`transmit` 它只是傀儡，最终交给 ActionCable.connection 执行。

在这里，`reject` 和 `transmit` 就像 Controller 里 `render` 和 `redirect_to` 一样的东西。

### Streams

```ruby
# pubsub.subscribe
stream_from

# pubsub.unsubscribe
stop_all_streams

# 封装 stream_from
stream_for
```

> Note：由 `pubsub` 完成实际操作。

```ruby
class CommentsChannel < ApplicationCable::Channel
  def follow(data)
    stream_from "comments_for_#{data['recording_id']}"
  end

  def unfollow
    stop_all_streams
  end
end

CommentsChannel.broadcast_to(@post, @comment)
```

Connection 终于有了自己的一点处理能力，虽然小，但你可以选择使用，也可放弃。

常用 `stream_from` 以处理一对一消息；`stop_all_streams` 单方面即可中止服务。`stream_for` 和 `stream_from` 类似，只是多了一个空间。

```
stream_from -> connection.transmit -> Connection#transmit -> WebSocket#transmit
```

另：

```
@websocket = ::WebSocket::Driver.websocket?(env) ? ClientSocket.new(env, event_target, stream_event_loop) : nil
```
