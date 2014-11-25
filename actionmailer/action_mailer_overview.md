## 其它

Action Mailer 使用模板来创建邮件与 Action Controller 使用模板渲染视图，原理类似。

Action Mailer 提供我们 mailer 类和视图，mailer 类和 controller 非常相似。它们继承于 ActionMailer::Base 并放在 app/mailers 目录下，它们有自己关联的视图文件在 app/views 目录下。

---

ActionMailer::Base 继承于类 AbstractController::Base,
又包含但不限于以下'外部'模块，根据 Ruby 规则，它们也是可调用的。包括：

| 模块 | 作用 |
| -- | -- |
| ActionMailer::DeliveryMethods | 邮件发送 |
| ActionMailer::Previews | 邮件预览 |
| AbstractController::Rendering | 渲染 |
| AbstractController::Helpers | 引进、输出辅助方法 |
| AbstractController::Translation | I18n 相关 |
| AbstractController::Callbacks | 支持回调 |
| ActionView::Layouts | 视图布局等 |

`Dar < Car < Bar` 

有时候 Car 要 extend 前人 Bar，并不是为了 Car 自己使用，而是为了方便后人 Dar. ActionMailer::Base 部分代码充当的角色和 Car 一样，它自己却没有使用，而是为了方便我们自定义的 Mailer 类调用。

如：

Mailer 里：

```ruby
before_action :add_inline_attachment!

layout "mailer"

helper :application
```

View 里：

```ruby
<%= url_for(host: "example.com", controller: "welcome", action: "greeting") %>

<%= users_url(host: "example.com") %>
```

**快速生成 Mailer 和模板**

通过 `rails g mailer UserMailer welcome` 创建 mailer 类和视图：

```
create  app/mailers/user_mailer.rb
invoke  erb
create    app/views/user_mailer
create    app/views/user_mailer/welcome.text.erb
invoke  test_unit
create    test/mailers/user_mailer_test.rb
```

**创建邮件对象时的魔法**

细心的你应该发现，我们在 Mailer 类里定义的是实例方法，但创建 mailer 对象用的却是类方法。

这里隐藏着魔法，当找不到此类方法时，就会调用 Rails 重新实现的 method_missing 类方法, 会先检查 action_methods 里是否有同名方法，如果有，则(把此方法当做参数对待)创建 Mailer 对象。
