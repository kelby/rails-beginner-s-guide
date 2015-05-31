## 其它

### 更多关于 Action Controller

Controller 里的 public方法(也就是action) 会自动对应 Route 里的路由规则。当请求到来时，action 接受请求并处理，最后渲染相应视图模板(Get-and-show)或重定向到另一 action(do-and-redirect).

默认，只有 ApplicationController 直接继承于 ActionController::Base，其它的控制器继承于 ApplicationController. 所以，如果你想在所有 controller 处理之前做一些什么，你可以把它们写在 ApplicationController 里。

ActionController include 了对 metal/ 目录下面的模块，而我们自定义的 Controller 又继承于 ActionController.

自然的，它们的 ClassMethods 就会变成我们自定义 Controller 的类方法，而其它方法则类似实例方法，可运用于 action.

### 运行 action 时的魔法

请求从 Action Dispatch 的 Routes 里转发过来，首先到达 Metal 的 self.action 方法，然后经过层层 middleware 处理。<br>
再然后调用 Metal 的 dispatch 方法(这里会创建 Metal 的实例对象)，调用 AbstractController::Base 的 process --> process_action --> send_action & send 完成。

### Railties Helpers

根据配置及其它因素，决定是否给我们所定义的 Controller 加载所有可用的 helpers.

也就是：

```ruby
klass.helper :all
```

> Note: 可通过 config.action_controller.include_all_helpers 进行配置只加载 application_helper 和当前 Controller 所对应的 x_helper.
