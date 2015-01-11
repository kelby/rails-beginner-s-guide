## 其它

### 更多关于 Action Controller

ActionController include 了对 metal/ 目录下面的模块，而我们自定义的 Controller 又继承于 ActionController.

自然的，它们的 ClassMethods 就会变成我们自定义 Controller 的类方法，而其它方法则类似实例方法，可运用于 action.

### 运行 action 时的魔法

请求从 Action Dispatch 的 Routes 里转发过来，首先到达 Metal 的 self.action 方法，然后经过层层 middleware 处理。<br>
再然后调用 Metal 的 dispatch 方法(这里会创建 Metal 的实例对象)，调用 AbstractController::Base 的 process --> process_action --> send_action & send 完成。

### Railties Helpers

根据配置及其它因素，决定是否给我们所定义的 Controller 加载所有可用的 helpers.

也就是：

```
klass.helper :all
```

> Note: 可通过 config.action_controller.include_all_helpers 进行配置只加载 application_helper 和当前 Controller 所对应的 x_helper.

### Railtie

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
