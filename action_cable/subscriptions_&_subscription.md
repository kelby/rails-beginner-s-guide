### Subscription

#### Subscription

传递消息，以及真正干活的好手。

**传递消息**

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

**真正干活**

我们可以(和普通 JS 一样)在它的基础上自定义方法，进行调用。

