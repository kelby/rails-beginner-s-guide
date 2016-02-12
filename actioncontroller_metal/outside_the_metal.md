## Metal 的增强模块

有一些模块和 Metal 一样也有 `process_action` 方法，并且它们也被 include 进了 Action Controller，并且我们自己定义的类没有重写这个方法。

根据 Ruby 的代码执行规则，执行 process_action 时它们都会被执行(并且这个方法执行顺序先于 Metal 同名的方法)。

源代码里，Metal 仅代表 metal.rb 这个文件，不包括与其同名的 metal 目录。

1) **继承 Abstract Controller 的财富**

由 Metal 到 Base 继承而来。

2) **使用 Action Dispatch 的资源**

主要是 Action Dispatch 的 http 和 middleware 下的内容。

包括但不限于：

处理 request、response 相关和 headers 相关。(Metal 也可以做，但这里得到了充分利用)

3) **协作 Action View**

有很多类似的方法或丝丝关联，比如：渲染。

4) **少量的 Active Model**

为了约定优于配置，很少的一部分方法和 Active Model 的 Naming 模块有关联。

包括但不限于：

```
wrap_parameters

polymorphic_url
polymorphic_path
```

5）处理请求，响应
