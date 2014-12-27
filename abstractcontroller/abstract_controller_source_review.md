## 源码导读

Abstract Controller 目前包含 10 个模块，部分在前面已经详细介绍了。

下面对所有的模块做简单描述，以有利于你对源码的阅读。

### Translation

I18n 相关的 `translate & t` 和 `localize & l` 方法。

### Rendering

我们自定义的 Controller#actions 里使用的 `render` 就是在这里定义的。

它其实是封装了 Action View 里的渲染相关内容，然后又被 Action Controller 所调用。

### Logger

提供 `logger` 方法打印日志。

### Helpers

不是指 helper 方法！但和 helper 有关联，如方法：helper_method、helper 等。

### Collector

响应格式 Mime.

### Callbacks

Controller 里的回调。

### Base

请求从 Route 到 Controller#actions 是如何转变的？魔法(答案)在这，从 process 到 _find_action_name、process_action ...

还有一些平时用得不多，但比较有趣的方法。

### Asset Paths

以声明的形式，定义一些 assset 相关的类方法和实例方法。

```
config_accessor :asset_host, :assets_dir, :javascripts_dir,
                :stylesheets_dir, :default_asset_host_protocol, :relative_url_root
```

### Routes Helpers

**引入 Route 相关的 helper**(这里只是调用，定义在 RouteSet 里)。

`routes.rb` 里定义的每一个路由规则都会有对应的 x_url 和 x_path 等 helper 方法可用，这里 include 了这些 helper.

然后，Action Controller 和 Action Mailer 的 Railtie 又 extend Routes Helpers，所以可用。

> Note: 可以通过 `include Rails.application.routes.url_helpers` 然后调用和 Routing 相关的 helper 方法。

### ~~Url For~~

包含了 ActionDispatch::Routing::UrlFor，然后又被 ActionMailer 和 ActionController::UrlFor 所调用。

这也是 Action Dispatch 和其它 3 个模块都有联系的证据之一。
