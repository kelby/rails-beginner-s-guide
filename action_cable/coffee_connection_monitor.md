### Connection Monitor

轮询机制，通过浏览器自带特性就能实现，非常重要的“约定”。

配置：

```ruby
@pollInterval:
  min: 3
  max: 30

@staleThreshold: 6
```

常用方法如：

```
connected
disconnected

received
```

其它：

```
identifier
constructor

reset
start
stop
poll

getInterval

reconnectIfStale

connectionIsStale
disconnectedRecently

visibilityDidChange

now

secondsSince

clamp
```

也是通过“死循环”实现的。
<br />
这里的“死循环”是有间隔、有状态、可控的。
