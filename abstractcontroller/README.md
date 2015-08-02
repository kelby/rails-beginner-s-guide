# Abstract Controller

服务于 Action Controller 和 Action Mailer.

- 辅助 ActionController::Base 将站场转移到具体的 Controller#action(经过 Metal).

- 辅助 ActionMailer::Base 将站场转移到具体的 Mailer#action.

Abstract Controller 起到了承上启下的作用，无论是它自己定义的方法，还是封装 Action View 和 Action Dispatch 得到的方法，最终都提供给 Action Controller 和 Action Mailer 使用。

这些方法(或模块)包含但不限于渲染(模板或局部模板)、Helpers、Callbacks、Mime、Url For 等。

Action Mailer 和 Action Controller 都继承于 Abstract Controller：

```ruby
ActionMailer::Base < AbstractController::Base

ActionController::Base < ActionController::Metal < AbstractController::Base
```
