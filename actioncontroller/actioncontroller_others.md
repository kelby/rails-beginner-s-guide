包含了：Railties Helpers、Railtie、Middleware 等。

## Railties Helpers

根据配置及其它因素，决定是否给我们所定义的 Controller 加载所有可用的 helpers.

也就是：

```
klass.helper :all
```

> Note: 可通过 config.action_controller.include_all_helpers 进行配置只加载 application_helper 和当前 Controller 所对应的 x_helper.

## Railtie

**initializer**

```
Assets config    # 配置 assets_dir，默认是 public/
Set helpers path # 默认是 app/helpers/
Parameters config
Set configs
Compile config methods
```

配置可通过以下方式查看：

```
Rails.configuration.action_controller

或

Rails.application.config.action_controller
```

默认配置项：

```ruby
Rails.configuration.action_controller.keys
# => [:perform_caching, :assets_dir, :logger, :cache_store, :javascripts_dir,
#     :stylesheets_dir, :asset_host, :relative_url_root]
```

## ~~Middleware~~

继承于 Metal，最初就是从 Metal 分离而来的。

这里的 Middleware<sup>1</sup> 不是 Middleware Stack 里面的 Middleware<sup>2</sup>，它们只是恰好同名而矣。

证明了 Metal 本身也是一个 Middleware<sup>2</sup>，本身就是一个 Rack app 等。
