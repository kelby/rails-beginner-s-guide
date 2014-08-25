# 源码导读

**UrlFor**

包含了 ActionDispatch::Routing::UrlFor，然后又被 ActionMailer 和 ActionController::UrlFor 所调用。

这也是 ActionDispatch 和其它 3 个模块都有联系的证据之一。

**Translation**

I18n 相关的 translate(alias t) 和 localize(alias l) 方法。

**Rendering**

我们自定义的 Controller#actions 里使用的 render 就是在这里定义的。

它其实是封装了 ActionView 里的渲染相关内容，然后又被 ActionController 所调用。

**Logger**

日志相关。提供 logger 打印日志

**Helpers**

Helper 相关方法。如：helper_method、helper.

**Collector**

响应格式 Mime.

**Callbacks**

Controller 里的回调。

**Base**

请求从 Route 到 Controller#actions 是如何转变的？魔法(答案)在这，从 process 到 _find_action_name、process_action ...

还有一些平时用得不多，但比较有趣的方法。

**AssetPaths**

以声明的形式，`定义`一些 assset 相关的类方法和实例方法。

**RoutesHelpers**

引入 Route 相关的 helper(这里只是调用，定义在 RouteSet 里)。`routes.rb` 里定义的每一个路由规则都会有对应的 xxx_url 和 xxx_path 等 helper 方法可用，这里 include 了这些 helper。然后，ActionController 和 ActionMailer 的 Railtie 又 extend RoutesHelpers，所以可用。

> **Note:** 通过 include Rails.application.routes.url_helpers 然后调用这些 helper 方法。
