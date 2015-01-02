## Hide Actions

Controller 里的 public method 根据约定都可以当做 action (特征之一：通过外面可调用)，可用这里的方法改变约定。

```
hide_action     # 设置 public method 为 hide，之后它就不会被当做 action 对待。

visible_action? # 判断 public method 是否被 hide.

action_methods  # action_methods 同名方法，把 hide_action 排除出来。
```
