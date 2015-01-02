**Reloader**

开发环境下，代码更改后，不用重启，自动加载新的代码。

用于是否缓存类和模块，如果不缓存，则每次请求都要加载类和模块。默认开发环境为 false，这样我们更改代码后，不需要重启就能看到效果。而测试、生产环境为 true.

对应 `config.cache_classes = false`

在每次请求之前，执行：run_callbacks :prepare
<br>
在每次请求之后，执行：run_callbacks :cleanup

注意，它只是调用接口。真正处理不是它完成的。

```
define_callbacks :prepare
define_callbacks :cleanup
```

用到了 File Update Checker.
