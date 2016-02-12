## Subscription Adapter

用于适配 ActionCable 的存储方案，默认是 Redis.

具体的存储方案，只要实现下列几个方法即可：

```
broadcast(channel, payload)

subscribe(channel, message_callback, success_callback = nil)
unsubscribe(channel, message_callback)

shutdown
```