### Rails 默认已有的 instrumenter

**action_mailer**

```
deliver.action_mailer
receive.action_mailer
process.action_mailer
```

**action_controller**

```
write_fragment.action_controller
read_fragment.action_controller
exist_fragment?.action_controller
expire_fragment.action_controller

start_processing.action_controller
process_action.action_controller
send_file.action_controller
send_data.action_controller
redirect_to.action_controller
halted_callback.action_controller

unpermitted_parameters.action_controller

deep_munge.action_controller
```

**action_view**

```
render_collection.action_view
render_partial.action_view

render_template.action_view
```

**active_job**

```
perform_start.active_job
perform.active_job

enqueue_at.active_job
enqueue.active_job
```

**active_record**

```
instantiation.active_record
sql.active_record
```

**active_support**

```
cache_read.active_support
cache_generate.active_support
cache_fetch_hit.active_support
cache_write.active_support
cache_delete.active_support
cache_exist?.active_support

cache_delete_matched.active_support

cache_increment.active_support
cache_decrement.active_support

cache_cleanup.active_support
cache_prune.active_support
```

**rails**

```
deprecation.rails
```

**railties**

```
load_config_initializer.railties
```

参考

[Active Support Instrumentation](http://edgeguides.rubyonrails.org/active_support_instrumentation.html)
