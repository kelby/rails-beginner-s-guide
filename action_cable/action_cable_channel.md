## Channel

常在，可直接调用。类似 Controller ... 外部请求过来，MVC 里最先由它处理。

### Base

它是 Channel 的头：

```ruby
      include Callbacks
      include PeriodicTimers
      include Streams
      include Naming
      include Broadcasting
```

**内部处理**

它结构比 Controller 简单，不需要经过 Abstract Controller, Action Controller, Metal 等重重处理。它有类方法 `action_methods`、实例方法 `perform_action` 和私有方法 `dispatch_action`

**对外提供接口**

a channel like a form of controller

but one that's capable of pushing content to the subscriber in addition to simply responding to the subscriber's direct requests.

Channel instances are long-lived. 

```
defer_subscription_confirmation!()
defer_subscription_confirmation?()

reject()

subscribed()
unsubscribed()

subscription_confirmation_sent?()
subscription_rejected?()

transmit(data, via: nil)
```

默认其子类要重写 `subscribed` 和 `unsubscribed` 方法。

`subscribed` 相当于初始化。

如果不允许订阅，可以用 `reject` 进行拒绝。

`transmit` 它只是傀儡，最终交给 ActionCable.connection 执行。

在这里，`reject` 和 `transmit` 就像 Controller 里 `render` 和 `redirect_to` 一样的东西。

### Streams

```
stream_from

stream_for

stop_all_streams
```

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

常用 `stream_from` 以处理一对一消息；`stop_all_streams` 单方面即可中止服务。
