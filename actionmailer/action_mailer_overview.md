ActionMailer 使用模板来创建邮件与 ActionController 使用模板渲染视图，原理类似。

ActionMailer 提供我们 mailer 类和视图，mailer 类和 controller 非常相似。它们继承于 ActionMailer::Base 并放在 app/mailers 目录下，它们有自己关联的视图文件在 app/views 目录下。

## Mail Helper

| 方法 | 解释 |
| -- | -- |
| mailer() | 表示邮件所在的 Mailer 对象，类似 Controller 对象 |
| message() | 表示邮件 |
| attachments() | 表示邮件里面的附件 |
|format_paragraph(text, len = 72, indent = 2)|处理一段文本消息，行首空两格，每行长度不超过 72 个字符|
|block_format(text)|使用 format_paragraph 处理大段的文本|

## Message Delivery

专用于邮件的发送。

这个文件是重构 + 引入 DeliveryJob 后产生的。

```
deliver_now, deliver_now!
deliver_later, deliver_later!

message
```

deliver 直接调用了 gem 'mail' 提供的方法，或加入延迟任务再调用。

通常，我们都是创建邮件并发送

```ruby
# Creates the email and sends it immediately
Notifier.welcome("helloworld@example.com").deliver_now
```

也可以先创建邮件对象，稍后邮件对象调用方法发送邮件：

```ruby
message = Notifier.welcome("helloworld@example.com") # => an ActionMailer::MessageDeliver object
message.deliver_now                                  # sends the email
```

再或者，自动延迟发送

```ruby
Notifier.welcome(david).message     # => a Mail::Message object
```

> NOTE: 现在 Rails 默认已经有延迟发送的方法。

## 其它

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

**生成 Mailer 和模板**

通过 `rails g mailer UserMailer welcome` 创建 mailer 类和视图：

```
create  app/mailers/user_mailer.rb
invoke  erb
create    app/views/user_mailer
create    app/views/user_mailer/welcome.text.erb
invoke  test_unit
create    test/mailers/user_mailer_test.rb
```

**邮件测试**

默认 Rails 提供两个 helper 方法用于测试：

|方法|解释|
|--|--|
|assert_emails(number) | 断言已经发送的邮件数|
|assert_no_emails(&block) | 断言没有邮件发送出去(可用 assert_emails 0 代替)|

assert_emails 和 assert_no_emails 两者本质都是封装 assert_equal.

**创建消息并渲染邮件模板**

**创建邮件对象**：细心的你应该发现，我们在 Mailer 类里定义的是实例方法，但创建 mailer 对象用的却是类方法。这里隐藏着魔法，当找不到此类方法时，就会调用 Rails 里重新定义的 method_missing, 找不到方法时先检查方法名是否和 action_methods 一样，如果一样则(把此方法当做参数对待)创建 Mailer 对象。

