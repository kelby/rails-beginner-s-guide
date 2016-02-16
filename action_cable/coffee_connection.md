### Connection

表面操作的是 connect，实质是直接操作 websocket ...

```
send

open
close
reopen

disconnect
```

它所打开、连接的就是 WebSocket 连接。

```
isOpen
isState

getState

installEventHandlers

events
```

不过我们很少直接使用它。
<br>
需要发消息的话，可以让 Subscription 调用，然后通过 Consumer 中转，最后才由 Connection 进行发送。
