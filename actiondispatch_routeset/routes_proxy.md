## ~~Routes Proxy~~

```
mount SomeRackApp, at: "some_route"
```

如果 mount 的 Engine 恰好也是一个 Rails app，那么就会生成带前缀的路由 helper 方法。

此时，Engine 内外，Engine 之间如何通信？通过代理(也就是 Routes Proxy 的实例对象)啊。
