#### Routes Helpers

**引入 Route 相关的 x_path 和 x_url 辅助方法。**

这里只是引入，这些方法在 RouteSet 里定义。

```ruby
include Rails.application.routes.url_helpers
```

之后，就能调用和 Routing 相关的 helper 方法。

`routes.rb` 里定义的每一个路由规则都会有对应的 x_url 和 x_path 等 helper 方法可用，这里 include 了这些 helper 方法。

然后，Action Controller 和 Action Mailer 的 Railtie 又 extend Routes Helpers，所以可用。
