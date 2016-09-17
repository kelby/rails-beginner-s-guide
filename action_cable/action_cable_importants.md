## 一些重要的东西

### ActionCable.server

Rails 默认使用的实例是 `ActionCable.server` 除了 Base 提供的实例方法外，它还可调用 Broadcasting 和 Connections 提供的方法。

### App.cable

Rails 默认使用 `App.cable` 表示其 Consumer 实例。

```ruby
@App = {}
App.cable = ActionCable.createConsumer()
```

### 术语

server 服务端（死的）

connection 链接本身（活的）

consumer 客户端（死的）

channels 链接通道（死的）

broadcastings 对通道的某些操作（活的）

### 流程

1）A输入数据，jQuery 监测，发送类似 Ajax 请求；服务器收到，服务器处理后，发送 ws 请求
1）或由服务端 server.broadcast 直接发送 ws 请求

发送的 ws 请求，要有标识及数据。

2）客户端始终在等待 ws 请求
3）客户端收到 ws 请求，根据标识由对应的 Channel js 处理

此处有分支

4）客户端直接使用 JQuery 处理，不需要再请求服务器
4）客户端 @perform 发送 ws 请求，对应 Channel rb 里的 action 处理，再响应...server broadcast 或 Streams；

结束。

received...B jQuery 收到数据，进行响应，反应出来到页面。

重要方法：客户端 perform, 服务端 broadcast 和 received.
