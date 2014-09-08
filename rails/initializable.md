# Initializable

指的是 Rails::Initializable

我们常用的 `initializer` 方法，就是它定义的。

被 Railtie 所调用，又由于继承关系，Engine、Application、YourApp 都可用。

## Instance Public methods

```
initializers
run_initializers
```

## ClassMethods

```
initializer, initializers, initializers_chain, initializers_for
```

## 其它

怎么存储，怎么运行？在此不再深入，感兴趣的可以自己看源代码。
