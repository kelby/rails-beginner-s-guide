## Initializable

指的是 Rails::Initializable.

我们常用的 `initializer` 方法，就是它定义的。

被 Railtie 所调用，又由于继承关系，Engine、Application、AppName 都可用。

类方法：

```
initializer
initializers_for

initializers
initializers_chain
```

实例方法：

```
initializers

run_initializers
```
