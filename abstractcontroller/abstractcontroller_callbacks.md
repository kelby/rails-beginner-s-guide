## Callbacks

Controller 里的回调。

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

skip_action_callback & skip_filter
```

这些回调方法参照物都是 `process_action` 同名方法。

```ruby
def process_action(*args)
  run_callbacks(:process_action) do
    super
  end
end
```

具体实现：调用了 ActiveSupport::Callbacks 里的 `define_callbacks`、`set_callback`、`skip_callback` 和 `run_callbacks` 方法。

上面 before, after, around 的 append, prepend, skip 类似，明白其中一个，其它的可以贯通。
skip_action_callback 意味着 skip: before, after, around.
