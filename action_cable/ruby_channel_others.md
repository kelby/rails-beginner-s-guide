## Channel 补充

### Broadcasting

类方法：

```
broadcast_to(model, message)

broadcasting_for
```

它只是傀儡，最终交给 `ActionCable.server` 执行。

### Callbacks

相关回调操作：

```
after_subscribe & on_subscribe
after_unsubscribe & on_unsubscribe

before_subscribe
before_unsubscribe
```

### Naming

```
channel_name
```

通过此方法，可以获取通道名字：

```ruby
ChatChannel.channel_name # => 'chat'
Chats::AppearancesChannel.channel_name # => 'chats:appearances'
```

### Periodic Timers

```
periodically(callback, every:)
```

可以在 Channel 里使用的类方法，周期性的执行某个指定的回调或 action.

算是一个配置，也不是真正执行。
