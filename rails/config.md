## Configuration Middleware Stack Proxy

我们常用的 `config.middleware`，其实是 Middleware Stack Proxy 的实例对象。对应 Rails::Configuration::MiddlewareStackProxy

被 Railtie 所调用，又由于继承关系，Engine、Application、YourApp 都可用。

**实例方法：**

`use`

```
config.middleware.use Magical::Unicorns
```

`insert_before & insert`

```
config.middleware.insert_before ActionDispatch::Head, Magical::Unicorns
```

`insert_after`

```
config.middleware.insert_after ActionDispatch::Head, Magical::Unicorns
```

`swap`

```
config.middleware.swap ActionDispatch::Flash, Magical::Unicorns
```

`delete`

```
config.middleware.delete ActionDispatch::Flash
```

除以上方法外，还有：

`unshift`

运用上述方法，我们可以增加、删除 middleware，或改变 Middleware 顺序。
