## Connection

### Base

这里的"连接"几乎等价于 Websocket 连接。

```ruby
# 简单封装 transmit
beat

close

# 不要直接调用，应由 connect、disconnect 触发
process

receive

send_async

statistics

# 不要直接调用，应由 Channel 实例对象的 transmit 触发
transmit
```

其子类里的 public 实例方法，可直接调用。

注意：可以重写（覆盖）父类的方法，但区别于 public 实例方法，不能直接调用。

```ruby
class AppearanceChannel < ApplicationCable::Channel
  def subscribed
    @connection_token = generate_connection_token
  end

  def unsubscribed
    current_user.disappear @connection_token
  end

  def appear(data)
    current_user.appear @connection_token, on: data['appearing_on']
  end

  def away
    current_user.away @connection_token
  end

  private
    def generate_connection_token
      SecureRandom.hex(36)
    end
end
```

示例代码中，`subscribed` 和 `unsubscribed` 由父类定义，不能直接调用。`generate_connection_token` 私有方法，不能直接调用。
<br>
`appear` 和 `away` 可以直接调用。

其它方法：

```ruby
attr_reader :server, :env, :subscriptions, :logger, :worker_pool
delegate :event_loop, :pubsub, to: :server
```

另外，它提供的一些 protected 方法也很实用：

```
request

cookies
```

这里的 `request` 是 ActionDispatch::Request 的实例对象，所以通过它也可以获取到一些和请求有关的数据。

### Identification

类方法：

```
identified_by
```

常用 `identified_by` 进行身份验证。

实例方法：

```
connection_identifier
```

### Authorization

```
reject_unauthorized_connection
```

常用 `reject_unauthorized_connection` 进行报错，只能被动地等着被调用。
