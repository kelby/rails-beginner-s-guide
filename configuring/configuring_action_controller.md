### Action Controller

FalseClass

```ruby
# 是否启用 Fragment 缓存
:perform_caching
```

String

```
:assets_dir
:javascripts_dir
:stylesheets_dir
```

ActiveSupport::Logger

```
:logger
```

ActiveSupport::Cache::NullStore

```
:cache_store
```

NilClass

```
:asset_host
:relative_url_root
```

TrueClass

```ruby
:forgery_protection_origin_check

# Helper 方法在所有 View 里都可用
:include_all_helpers

per_form_csrf_tokens
```

其它：

```
config.action_controller.asset_host

config.action_controller.default_static_extension

config.action_controller.default_charset

config.action_controller.logger

config.action_controller.request_forgery_protection_token

config.action_controller.allow_forgery_protection

config.action_controller.permit_all_parameters

config.action_controller.action_on_unpermitted_parameters

config.action_controller.always_permitted_parameters
```
