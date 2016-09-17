## Rails 默认组件都是 Railtie

#### Action Mailer

初始化：

```
logger

set_configs

compile_config_methods
```

#### Abstract Controller

**引入 Route 相关的 helper**(这里只是调用，定义在 RouteSet 里)。

`routes.rb` 里定义的每一个路由规则都会有对应的 x_url 和 x_path 等 helper 方法可用，这里 include 了这些 helper.

然后，Action Controller 和 Action Mailer 的 Railtie 又 extend Routes Helpers，所以可用。

> Note: 可以通过 `include Rails.application.routes.url_helpers` 然后调用和 Routing 相关的 helper 方法。

#### Action Controller

**initializer**

```bash
Assets config    # 配置 assets_dir，默认是 public/
Set helpers path # 默认是 app/helpers/
Parameters config
Set configs
Compile config methods
```

配置可通过以下方式查看：

```ruby
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

#### Action Dispatch

初始化 configure 及其它。

#### Action View

```
embed authenticity token in remote forms.

logger.

set configs.

caching.

setup action pack.
```

#### Active Model

加载 Action Model 相关 I18n

```ruby
ActiveSupport.on_load(:i18n) do
  I18n.load_path << File.dirname(__FILE__) + '/active_model/locale/en.yml'
end
```

#### Active Record

获取 database.yml 的配置信息：

```ruby
Rails.application.config.database_configuration
```

... ...

#### Active Job

```

```

另，配置默认 queue_adapter 为 :inline

#### Active Support

```ruby
active_support.deprecation_behavior
active_support.initialize_time_zone
active_support.initialize_beginning_of_week
active_support.set_configs
```

#### I18n

after_initialize 和 before_eager_load 都执行 initialize_i18n
