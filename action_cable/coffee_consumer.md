### 第零梯队 & 第一梯队 Consumer

由 Consumer 根据 action-cable-url 得到 url (使用协议 ws)，创建 WebSocket 实例。

一并创建 Subscriptions、Connection 及 ConnectionMonitor 实例。
<br>
这几个实例之间要互相操作的话，通过 Consumer 实例完成。

```
constructor

send

inspect

toJSON
```
