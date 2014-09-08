# Configuration

Railtie、Engine、Application 都有自己的 Configuration 模块。

由于 Ruby 的继承机制，我们常用的 `config` 可以看作是它们中任意一个的实例对象。所以，理论上来说，它们提供的接口 `config` 都可直接调用。

## 对外提供接口

```
autoload_once_paths, autoload_paths
eager_load_paths
middleware
paths
root=
```
