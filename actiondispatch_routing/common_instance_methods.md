### 路由常用方法汇总

下面方法都可以看做是 `Mapper` 的实例方法，如果我们想要定制自己的方法，参考它们即可。

基本：

```
root

mount

match
```

Http 方法：

```
get
post
patch
put
delete
```

重定向：

```
redirect
```

作用域：

```
scope
controller
namespace
constraints
defaults
```

关联：

```
concern
concerns
```

资源：

```
resource
resources
collection
member
```

常用 gem 'devise' 就是通过给 `Mapper` 增加实例方法的方式，在路由里为我们提供了  `devise_for` 和 `devise_scope` 方法。
