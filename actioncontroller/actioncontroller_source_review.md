## Railties Helpers

根据配置及其它因素，决定是否给我们所定义的 Controller 加载所有可用的 helpers.

也就是：

```
klass.helper :all
```

> Note: 可通过 config.action_controller.include_all_helpers 进行配置只加载 application_helper 和当前 Controller 所对应的 x_helper.
