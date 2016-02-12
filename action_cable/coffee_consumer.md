### 第一梯队 Consumer

由 Consumer 根据 action-cable-url 得到 url (使用协议 ws)，创建 WebSocket 实例。

```
constructor
```

一并创建 Subscriptions、Connection 及 ConnectionMonitor 实例。

```
send
```

希望 @webSocket 发送消息，通过 Consumer 实例完成。


