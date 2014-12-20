## Railtie

### initializer

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
