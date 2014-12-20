## AppName, Application, Engine, Railtie

从上层到底层。

AppName < Application < Engine < Railtie

最直观的就是文件、目录结构，以及配置文件。其次，是默认组件。

### 默认组件都是 Railtie

active_record, action_controller, action_view,  action_mailer, rails/test_unit, sprockets 还有 active_model 都属于 Railtie.

### 查看配置有哪些 eager_load_namespaces

```ruby
Rails.configuration.eager_load_namespaces

=> [ActiveSupport,
 ActionDispatch,
 ActiveModel,
 ActionView,
 ActionController,
 ActiveRecord,
 ActionMailer,

 Coffee::Rails::Engine,
 Jquery::Rails::Engine,
 Turbolinks::Engine,
 WebConsole::Engine,

 YourAppName::Application]
```

### 查看应用有哪些 Initializer

```ruby
Rails.application.initializers

=> [:load_environment_hook,
 :load_active_support,
 :set_eager_load,
 :initialize_logger,
 :initialize_cache,
 :initialize_dependency_mechanism,
 :bootstrap_hook,
 # 以上来自 Bootstrap

 "active_support.deprecation_behavior",
 "active_support.initialize_time_zone",
 "active_support.initialize_beginning_of_week",
 "active_support.set_configs",
 # 以上来自 Active Support

 "action_dispatch.configure",
 # 以上来自 Action Dispatch

 "active_model.secure_password",
 # 以上来自 Active Model

 "action_view.embed_authenticity_token_in_remote_forms",
 "action_view.logger",
 "action_view.set_configs",
 "action_view.caching",
 "action_view.setup_action_pack",
 # 以上来自 Action View

 "action_controller.assets_config",
 "action_controller.set_helpers_path",
 "action_controller.parameters_config",
 "action_controller.set_configs",
 "action_controller.compile_config_methods",
 # 以上来自 Action Controller

 "active_record.initialize_timezone",
 "active_record.logger",
 "active_record.migration_error",
 "active_record.check_schema_cache_dump",
 "active_record.set_configs",
 "active_record.initialize_database",
 "active_record.log_runtime",
 "active_record.set_reloader_hooks",
 "active_record.add_watchable_files",
 # 以上来自 Active Record

 "global_id",
 # 以上来自 gem 'globalid'
 # Active Job 依赖于它

 "active_job.logger",
 "active_job.set_configs",
 # 以上来自 Active Job

 "action_mailer.logger",
 "action_mailer.set_configs",
 "action_mailer.compile_config_methods",
 # 以上来自 Action Mailer

 :setup_sass,
 :setup_compression,
 # 以上来自 gem 'sass-rails'

 :jbuilder,
 # 以上来自 gem 'jbuilder'

 :set_load_path,
 :set_autoload_paths,
 :add_routing_paths,
 :add_locales,
 :add_view_paths,
 :load_environment_config,
 :append_assets_path,
 :prepend_helpers_path,
 :load_config_initializers,
 :engines_blank_point,
 # 以上来自 Engine
 
 # 来自 Engine，略...

 :turbolinks,
 # 以上来自 gem 'turbolinks'

 "web_console.initialize_view_helpers",
 "web_console.add_default_route",
 "web_console.process_whitelisted_ips",
 "web_console.process_command",
 "web_console.process_colors",
 # 以上来自 gem 'web_console'

 # 来自 Engine，略...

 :initialize_dependency_mechanism,
 # 以上来自 Bootstrap

 :add_generator_templates,
 :ensure_autoload_once_paths_as_subset,
 :add_builtin_route,
 :build_middleware_stack,
 :define_main_app_helper,
 :add_to_prepare_blocks,
 :run_prepare_callbacks,
 :eager_load!,
 :finisher_hook,
 :set_routes_reloader_hook,
 :set_clear_dependencies_hook]
 # 以上来自 Finisher
```
