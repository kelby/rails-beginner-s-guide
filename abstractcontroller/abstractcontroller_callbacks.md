## Callbacks

Controller 里可用的回调：

```ruby
before_action & append_before_action
prepend_before_action
skip_before_action

after_action & append_after_action
prepend_after_action
skip_after_action

around_action & append_around_action
prepend_around_action
skip_around_action

skip_action_callback & skip_filter # 将被废除
```

这些回调方法参照物都是 `process_action` 方法。

```ruby
set_callback(:process_action, callback, name, options)

set_callback(:process_action, callback, name, options.merge(:prepend => true))

skip_callback(:process_action, callback, name, options)
```

具体实现：调用了 ActiveSupport::Callbacks 里的 `define_callbacks`、`set_callback`、`run_callback` 和 `skip_callbacks` 方法。

另：skip_action_callback 意味着 skip before、after 和 around.
