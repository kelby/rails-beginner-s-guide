## ~~Digestor~~

**对视图进行加密，是片段缓存的组成部分。**

类方法：

```
digest

tree
```

目前，Rails 里有两个地方调用了它。

1)
Cache Helper 会调用。(也就是说调用 cache helper 方法时，会调用到它)

2)
Action Controller 里的 Etag With Template Digest 也会调用。(可配置将模板 digest 的结果放到 etaggers 里)

实例方法：

```
digest
dependencies

nested_dependencies
```

`dependencies` 会用到 Dependency Tracker.

> Note: 有类方法 digest 和同名实例方法 digest. 一般我们直接调用的是类方法，也就是 ActionView::Digestor.digest，之后它内部处理，调用实例方法。
