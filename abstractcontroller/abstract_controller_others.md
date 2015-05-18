## 其它

Abstract Controller 目前包含 10 个模块，部分在前面已经详细介绍了。

下面对所有的模块做简单描述，以有利于你对源码的阅读。

### Translation

I18n 相关的 `translate & t` 和 `localize & l` 方法。

区别于 Active Model 的 Translation 模块。

### Asset Paths

以声明的形式，定义一些 assset 相关的类方法和实例方法。

```ruby
config_accessor :asset_host, :assets_dir, :javascripts_dir,
                :stylesheets_dir, :default_asset_host_protocol, :relative_url_root
```

### Routes Helpers

**引入 Route 相关的 x_path 和 x_url 辅助方法。**

这里只是引入，这些方法在 RouteSet 里定义。

```
include Rails.application.routes.url_helpers
```

之后，就能调用和 Routing 相关的 helper 方法。

`routes.rb` 里定义的每一个路由规则都会有对应的 x_url 和 x_path 等 helper 方法可用，这里 include 了这些 helper.

然后，Action Controller 和 Action Mailer 的 Railtie 又 extend Routes Helpers，所以可用。

### ~~Logger~~

提供 `logger` 方法打印日志。

### ~~Url For~~

包含了 Action Dispatch，然后让 Action Mailer 和 Action Controller 调用。再具体一点是：简单封装了 ActionDispatch::Routing::UrlFor，然后给 Active Mailer 和 ActionController::UrlFor 使用。

(在这里 Action Controller 和 Action Mailer 不直接与 Action Dispatch 沟通)

这也是 Action Dispatch 和其它 3 个模块都有联系的证据之一。
