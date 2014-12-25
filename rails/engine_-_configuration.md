## Configuration

Railtie、Engine、Application 都有自己的 Configuration 模块。

由于 Ruby 的继承机制，我们常用的 `config` 可以看作是它们中任意一个的实例对象。所以，理论上来说，它们提供的接口 `config` 都可直接调用。

### 对外提供接口

```
paths
eager_load_paths
autoload_paths
autoload_once_paths

middleware

root
root=
```

除 root= 外，其余方法都是 get 获取数据，而不是 set 设置数据。

另，自定义的 Railtie 和自定义的 Engine，也可以对外提供 `config` 接口。
