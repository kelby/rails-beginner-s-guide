# Action Controller

本章节可分为三部分：Metal，Metal 之外，页面相关服务端缓存。

Controller 里的 public方法(也就是action) 会自动对应 Route 里的路由规则。当请求到来时，action 接受请求并处理，最后渲染相应视图模板(Get-and-show)或重定向到另一 action(do-and-redirect).

默认，只有 ApplicationController 直接继承于 ActionController::Base，其它的控制器继承于 ApplicationController. 所以，如果你想在所有 controller 处理之前做一些什么，你可以把它们写在 ApplicationController 里。

### 概念上，Metal 很重要

属于 Rack application (或者称之为 middleware).

可直接继承使用。

middleware_stack (这个很重要，最终指向 ActionDispatch::MiddlewareStack).

self.action + dispatch

ActionController::Metal 本身是一个功能极简的 Controller, 但它符合 Rack 接口规范，所以也可以把它称之为 Rack application. 相比于 ActionController::Base 它的功能真的很有限。

如你的 Rails 项目对性能要求比较高，或者说对实时性要求比较高，又或者你的项目做为 API 对外提供服务，那么你可以尝试直接继承使用 Metal.

Rails Metal is a subset of Rack middleware 可以参考【Rack - Ruby Web server 接口】章节了解更多关于 Rack 的内容。

### 论起实际意义，Metal 之外才是真正重要的

有一些模块和 Metal 一样也有 `process_action` 方法，并且它们也被 include 进了 Action Controller，并且我们自己定义的类没有这个方法(没有重写)。根据 Ruby 的代码执行规则，执行 process_action 时它们都会被执行(并且这个方法执行顺序先于 Metal 同名的方法)。

Metal 仅包含 metal.rb 这个文件，不包含其同名目录。

1) **继承 Abstract Controller 的遗产**

直接继承而来。

2) **使用 Action Dispatch 的资源**

主要是 Action Dispatch 的 http 和 middleware 下的内容。

包括但不限于：

处理 request、response 相关和 headers 相关。(Metal 也可以做，但这里得到了发扬)

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

### 页面相关服务端缓存，在这里比较独立

片段缓存相关，在这里可以单独拿出来讲。
