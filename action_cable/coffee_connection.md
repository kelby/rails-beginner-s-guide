### 第二梯队 Connection

操作的 connect，其实就是直接操作 websocket ...

一般地，我们不会直接操作它。

```
send
open
close
reopen

isOpen
isState
getState

installEventHandlers

events

disconnect

toJSON
```

它所打开的是 HTML5 自带就有的 WebSocket.
