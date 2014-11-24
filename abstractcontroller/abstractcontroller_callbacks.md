## Callbacks

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

具体实现：调用 ActiveSupport::Callbacks 的 `set_callback` 和 `skip_callback` 等方法。

上面 before, after, around 的 append, prepend, skip 类似，明白其中一个，其它的可以贯通。
skip_action_callback 意味着 skip: before, after, around
