## Connection 补充

### ~~Internal Channel~~

空

### ~~Message Buffer~~

```
append(message)

process!()

processing?()
```

### WebSocket

```
alive?()

possible?()

transmit(data)
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
```

```
log(type, message)
```
