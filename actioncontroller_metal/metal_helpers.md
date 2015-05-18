## Helpers

```
helpers
```

本质是 ActionView::Base 的实例对象：

```ruby
ApplicationController.helpers.class
# => ActionView::Base
```

rails 控制台里的 `helper` 方法指的就是它。

除上述方法外，还有：

```
all_helpers_from_path

helper_attr

modules_for_helpers
```

`helper_attr` 用到了 AbstractController::Helpers 里的 helper_method 方法。

另外，有配置：

```ruby
# 默认为 true
config.action_controller.include_all_helpers = false
```

可以设置单个 Controller 只加载和它名字对应的 Helper 模块，而不是所有的 Helper(所有的 helper 方法)。
