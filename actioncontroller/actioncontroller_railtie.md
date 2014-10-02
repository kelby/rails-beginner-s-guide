# ActionController Railtie

## initializer

```
Assets config
Set helpers path
Parameters config
Set configs
Compile config methods
```

配置可通过 `Rails.configuration.action_controller` 或 `Rails.application.config.action_controller` 查看。

默认配置项：

```ruby
Rails.configuration.action_controller.keys
# => [:perform_caching, :assets_dir, :logger, :cache_store, :javascripts_dir, :stylesheets_dir, :asset_host, :relative_url_root]
```
