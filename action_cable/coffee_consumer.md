### Consumer

**创建各个相关的实例对象**

由 Consumer 根据 action-cable-url 得到 url (使用协议 ws)，创建 WebSocket 实例。

```
constructor
```

一并创建 Subscriptions、Connection 及 Connection Monitor 实例。

**并间接具有可以发送 @webSocket 消息的能力。**

```
send
```

想让 @webSocket 发送消息，通过 Consumer 实例完成。

