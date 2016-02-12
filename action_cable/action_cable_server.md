## Server

Rails 默认使用的实例是 `ActionCable.server` 除了 Base 提供的实例方法外，它还可调用 Broadcasting 和 Connections 提供的方法。

### Broadcasting

send messages to the channel subscribers

```
broadcast

broadcaster_for
```

常用 `broadcast` 进行广播。

