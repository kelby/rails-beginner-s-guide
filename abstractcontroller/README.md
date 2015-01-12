# Abstract Controller

- 服务于 Action Controller 和 Action Mailer.
- 辅助 ActionController::Base 和 ActionMailer::Base 将站场转移到具体的 Controller#action(经过 Metal) 或 Mailer#action.

Abstract Controller 无论是它自己定义的方法，还是封装 Action View 和 Action Dispatch 得到的方法，最终都提供给 Action Controller 和 Action Mailer 使用。  
这些方法(或模块)包含但不限于渲染(模板或局部模板)、Helper相关、回调、Mime、Url For 等。

Abstract Controller 起到了承上启下的作用(所以，有时候它的代码会让你觉得很迷惑)，及 Action Dispatch 到 Action Controller 转换，并且也实现了一些重要的方法，如：Callbacks、Helpers.

Action Mailer 和 Action Controller 都继承于 Abstract Controller：

```
ActionMailer::Base < AbstractController::Base

ActionController::Base < ActionController::Metal < AbstractController::Base
```

Abstract Controller 还和 Action Dispatch 以及 Action View 有关联，会直接或间接调用它们的方法。
