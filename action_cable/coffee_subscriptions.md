### Subscriptions

Subscription 的集结地，有专门的 `@subscriptions` 用来保存 subscription.

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

还有各种方法，针对具体某个 subscription 进行各种操作。可订阅某个 Channel，之后在其 Channel 发布消息时，它就能收到了。

通过 identifier 查找具体某个 subscription.

