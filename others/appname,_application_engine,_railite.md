上层从到底层。

AppName < Application < Engine < Railtie

最直观的就是文件、目录结构，以及配置文件。其次，是默认组件。

## 默认组件都是 Railtie

active_record, action_controller, action_view,  action_mailer, rails/test_unit, sprockets 还有 active_model 都属于 Railtie.

## Configuration

eager_load_namespaces

```ruby
Rails.configuration.eager_load_namespaces
# =>
[ActiveSupport,
 ActionDispatch,
 ActiveModel,
 ActionView,
 ActionController,
 ActiveRecord,
 ActionMailer,
 Coffee::Rails::Engine,
 Jquery::Rails::Engine,
 Turbolinks::Engine,
 AppName::Application]
```

## Initializer

```
Rails.application.initializers

# =>
load_environment_hook
load_active_support

set_eager_load

initialize_logger
initialize_cache
initialize_dependency_mechanism

bootstrap_hook
# 以上 Bootstrap

active_support.deprecation_behavior
active_support.initialize_time_zone
active_support.initialize_beginning_of_week
active_support.set_configs
# 以上  ActiveSupport

action_dispatch.configure
# 以上 ActionDispatch

active_model.secure_password
# 以上 ActiveModel

action_view.embed_authenticity_token_in_remote_forms
action_view.logger
action_view.set_configs
action_view.caching
action_view.setup_action_pack
# 以上 ActionView

action_controller.assets_config
action_controller.set_helpers_path
action_controller.parameters_config
action_controller.set_configs
action_controller.compile_config_methods
# 以上 ActionController

active_record.initialize_timezone
active_record.logger
active_record.migration_error
active_record.check_schema_cache_dump
active_record.set_configs
active_record.initialize_database
active_record.log_runtime
active_record.set_reloader_hooks
active_record.add_watchable_files
# 以上 ActiveRecord

action_mailer.logger
action_mailer.set_configs
action_mailer.compile_config_methods
# 以上 ActionMailer

## 以下为其它
setup_sass
setup_compression
# 以上 sass-rails

set_load_path
set_autoload_paths

add_routing_paths
add_locales
add_view_paths

load_environment_config
append_assets_path
prepend_helpers_path
load_config_initializers
engines_blank_point
# 以上 Engine 1

set_load_path
set_autoload_paths

add_routing_paths
add_locales
add_view_paths

load_environment_config
append_assets_path
prepend_helpers_path
load_config_initializers
engines_blank_point
# 以上 Engine 2

set_load_path
set_autoload_paths

add_routing_paths
add_locales
add_view_paths

load_environment_config
append_assets_path
prepend_helpers_path
load_config_initializers
engines_blank_point

turbolinks
# 以上 Engine 3

set_load_path
set_autoload_paths

add_routing_paths
add_locales
add_view_paths

load_environment_config
append_assets_path
prepend_helpers_path
load_config_initializers
engines_blank_point

initialize_dependency_mechanism
# 以上 Engine 4
## 以上为其它

add_generator_templates
ensure_autoload_once_paths_as_subset
add_builtin_route
build_middleware_stack
define_main_app_helper
add_to_prepare_blocks
run_prepare_callbacks
eager_load!
finisher_hook

set_routes_reloader_hook
set_clear_dependencies_hook
# 以上 Finisher
```

