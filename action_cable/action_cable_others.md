## Action Cable others


### 命令行生成

generate 生成两文件，"channel.rb" 和 "assets/channel.coffee"

生成的 .rb 文件有默认方法：subscribed 和 unsubscribed，及其它自定义的方法。

生成的 coffee 文件有默认操作：链接、断开链接、接收到数据，及其它自定义操作。

### 依赖

自身：

```
  s.add_dependency 'actionpack', version
```

其它：

```
  s.add_dependency 'coffee-rails',     '~> 4.1.0'
  s.add_dependency 'faye-websocket',   '~> 0.10.0'
  s.add_dependency 'websocket-driver', '~> 0.6.1'
  s.add_dependency 'celluloid',        '~> 0.17.2'
  s.add_dependency 'em-hiredis',       '~> 0.3.0'
  s.add_dependency 'redis',            '~> 3.0'
```

### Engine

实现方式是：Engine
