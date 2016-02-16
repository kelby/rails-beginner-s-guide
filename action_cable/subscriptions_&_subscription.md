### Subscription & Subscriptions

#### Subscription

**传递消息以及干活的好手。**

常用的：

```ruby
# 封装 send
perform
```

其它：

```ruby
# 实质是 consumer.send -> connection.send
send
```

```
unsubscribe
```

创建 subscription 需要 channel.

常用方法 `perform` 可以在 Coffee 里调用 Channel 里的 action.

```
Subscription#perform -> Subscription#send -> Consumer#send -> Connection#send -> WebSocket#send
```

此外，我们还可(和普通 JS 一样)在它的基础上自定义方法，进行调用。

#### Subscriptions

常用的：

```
create
```

通过它可创建 Subscription 实例。

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
```

有专门的 `@subscriptions` 用来保存 subscription.

还有各种方法，针对具体某个 subscription 进行各种操作。可订阅某个 Channel，之后在其 Channel 发布消息时，它就能收到了。

通过 identifier 查找具体某个 subscription.

