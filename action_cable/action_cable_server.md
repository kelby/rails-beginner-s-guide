## Server

Rails 默认使用的实例是 `ActionCable.server` 除了 Base 提供的实例方法外，它还可调用 Broadcasting 和 Connections 提供的方法。

### Base

实例对象有：`ActionCable.server`

实例方法：

```ruby
# 所有 Channel 类
channel_classes

# identified_by 指定的标识符
connection_identifiers

# streams/broadcasting 所使用的适配器
# ActionCable::SubscriptionAdapter::Async 实例
pubsub

# ActionCable::RemoteConnections 实例
remote_connections

# ActionCable::Connection::StreamEventLoop 实例
stream_event_loop

# ActionCable::Server::Worker 实例
worker_pool

# 断开带有某标识符的连接
disconnect
```

```ruby
# ActionCable::Server::Configuration 实例
config
```

类方法：

```
logger
```

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

实际上广播任务由 server 对应的 `pubsub` 来完成。

### 广播与接收一般要对应

ActionCable.server 用于广播，它包括标识及数据。

ApplicationCable::Channel 用于接收数据并处理。

