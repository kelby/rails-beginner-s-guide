### 第二梯队 Connection

表面操作的是 connect，实质是直接操作 websocket ...

```
send
open
close
reopen
```

```
isOpen
isState

getState

installEventHandlers

events

disconnect
```

它所打开的是 HTML5 自带就有的 WebSocket.
