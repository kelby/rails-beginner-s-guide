# Connection 其它


### Base

```
beat

close

process

receive

send_async

statistics

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

### Internal Channel

空

### Message Buffer

```
append(message)

process!()

processing?()
```

### WebSocket

```
alive?()

possible?()

transmit(data)
```

### Subscriptions

```
add(data)

execute_command(data)

identifiers()

perform_action(data)

remove(data)

remove_subscription(subscription)

unsubscribe_from_all()
```

### Tagged Logger Proxy

```
add_tags(*tags)

tag(logger)
```

```
log(type, message)
```
