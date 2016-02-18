## Connection 补充

### WebSocket

```
alive?()

possible?()

transmit(data)
```

### ~~Internal Channel~~

表示，带有同一 identifier 的 connection.

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

### ~~Client Socket~~

