## Channel 补充

### Broadcasting

```
broadcast_to(model, message)

broadcasting_for
```

它只是傀儡，最终交给 `ActionCable.server` 执行。

### Callbacks

相关操作：

```
after_subscribe
after_unsubscribe

before_subscribe
before_unsubscribe
```

### Naming

```
channel_name
```

当然，这没什么实质意义。

### Periodic Timers

```
periodically(callback, every:)
```

算是一个配置，也不是真正执行。
