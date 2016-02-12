### 第二梯队 Subscriptions & Subscription

#### Subscription

常用的：

```ruby
# 封装 send
perform
```

其它：

```ruby
# connection.send
send

unsubscribe
```

创建 subscription 需要 channel.

自己几乎什么也不能做，仅有的方法也是通过操作 @consumer 或 @subscriptions 达到目的。

常用方法 `perform` 可以在 Coffee 里调用 Channel 里的 action.

```
Subscription#perform -> Subscription#send -> Consumer#send -> Connection#send -> WebSocket#send
```

#### Subscriptions

常用的：

```
create
```

其它：

```
constructor

add
remove
reject
forget
reload
notify

findAll
notifyAll

sendCommand

record

toJSON
```

有专门的 `@subscriptions` 用来保存 subscription.

还有各种方法，针对具体某个 subscription 进行各种操作。可订阅某个 Channel，之后在其 Channel 发布消息时，它就能收到了。

通过 identifier 查找具体某个 subscription.

