# Configuration Middleware Stack Proxy

这里指的是 Rails::Configuration，名字起得不是很好。

我们常用的 `config.middleware` 方法，就是它定义的。

被 Railtie 所调用，又由于继承关系，Engine、Application、YourApp 都可用。

## 对外提供的接口

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
