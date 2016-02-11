## Action Cable others

### 命令行生成

创建项目时，已经默认自动生成：cable.coffee、channel.rb 和 connection.rb.

使用：

```
rails generate channel NAME [method method] [options]
```

生成两文件："name_channel.rb" 和 "name.coffee"

生成的 `.rb` 文件有默认方法：subscribed 和 unsubscribed，及其它自定义的方法。
<br>
生成的 `.coffee` 文件有默认操作：链接、断开链接、接收到数据，及其它自定义操作。

### 依赖

自身：

```ruby
  s.add_dependency 'actionpack', version
```

其它：

```ruby
  s.add_dependency 'nio4r',            '~> 1.2'
  s.add_dependency 'websocket-driver', '~> 0.6.1'
```

当然了，redis 也是必须的，尽量这里没有声明。

### Engine

实现方式是：Engine
