## Connection 补充

### WebSocket

```
alive?()

possible?()

transmit(data)
```

### Tagged Logger Proxy

```
add_tags(*tags)

tag(logger)

debug
info
warn
error
fatal
unknown
```

```
log(type, message)
```

### ~~Internal Channel~~

表示带有同一 identifier 的 connection，本质还是 Channel.

### ~~Message Buffer~~

```
append(message)

process!()

processing?()
```

### ~~Subscriptions~~

```
add(data)

execute_command(data)

identifiers()

perform_action(data)

remove(data)

remove_subscription(subscription)

unsubscribe_from_all()
```

### ~~Client Socket~~

### ~~Stream~~

### ~~Stream Event Loop~~

### ~~Faye Client Socket~~

### ~~Faye Event Loop~~

### ~~Subscriptions~~