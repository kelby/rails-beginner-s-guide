## Server

Rails 默认使用的实例是 `ActionCable.server` 除了 Base 提供的实例方法外，它还可调用 Broadcasting 和 Connections 提供的方法。

### Broadcasting

发送消息给指定 channel 的订阅者。

实例方法：

```
broadcast

broadcaster_for
```

常用 `broadcast` 进行广播。

```ruby
ActionCable.server.broadcast "web_notifications_1",
  { title: 'New things!', body: 'All shit fit for print' }
```