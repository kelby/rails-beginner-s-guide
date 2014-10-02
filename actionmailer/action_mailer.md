ActionMailer 使用模板来创建邮件与 ActionController 使用模板渲染视图，原理类似。

ActionMailer 提供我们 mailer 类和视图，mailer 类和 controller 非常相似。它们继承于 ActionMailer::Base 并放在 app/mailers 目录下，它们有自己关联的视图文件在 app/views 目录下。

## 邮件配置

以 Gmail 为例：

```ruby
# 邮件发送报错时，是否把错误信息发送给用户。开发环境下，可设置为 true
ActionMailer::Base.raise_delivery_errors = true

# 发送方式
ActionMailer::Base.delivery_method = :smtp
# 根据不同发送方式，做不同配置
ActionMailer::Base.smtp_settings = {
  :address => "smtp.gmail.com",
  :port    => 587,
  # 可以填自己的域
  :domain  => "example.com",
  :authentication => :plain,
  # 完整的邮箱地址
  :user_name      => "test@example.com",
  :password       => "passw0rd",
  :enable_starttls_auto => true
}
```

delivery_method 常见有 smtp(Mail::SMTP)、file(Mail::FileDelivery)、sendmail(Mail::Sendmail) 和 test(Mail::TestMailer)，也可以自定义。

## 生成 Mailer 和模板

通过 `rails g mailer UserMailer welcome` 创建 mailer 类和视图：

```
create  app/mailers/user_mailer.rb
invoke  erb
create    app/views/user_mailer
create    app/views/user_mailer/welcome.text.erb
invoke  test_unit
create    test/mailers/user_mailer_test.rb
```

### 设置默认值

```ruby
# app/mailers/user_mailer.rb
class UserMailer < ActionMailer::Base
  default from: "from@example.com"

  def welcome
    @greeting = "Hi"
    mail to: "to@example.org"
  end
end
```

`default(value = nil)` 是ActionMailer::Base提供的方法，用来设置 default_params，默认已经有

```ruby
default_params = {
  mime_version: "1.0",
  charset:      "UTF-8",
  content_type: "text/plain",
  parts_order:  [ "text/plain", "text/enriched", "text/html" ]
}
```

还有在 config/environments 目录下针对不同执行环境会有不同的邮件服务器设置：

```ruby
config.action_mailer.default_options = { from: "no-reply@example.org" }
```

前者对当前 Controller 及其子类有效，而后者对当前环境下所有 Controller 有效。除了使用的地方不同，导致作用域稍有不同外，两者本质是一样的。

```ruby
alias :default_options= :default
```

### 创建消息并渲染邮件模板

`mail(headers = {}, &block)` 可接收一个代码块做为参数，header(头部)可接受：

| 参数 | 含义 |
| :-- | -- |
| :subject | 主题 |
| :from | 发件人 |
| :to | 收件人 |
| :cc | 抄送 |
| :bcc | 密送 |
| :reply_to | 回邮地址 |
| :date |时间|

> NOTE: 想了解更多 header，点击 [Email#Header_fields](http://en.wikipedia.org/wiki/Email#Header_fields)

前面提到过，mailer 类和普通的 controller 类似，你可以渲染相应的模板，也可以传递实例变量给它们。例如：

```ruby
def welcome(user)
  @user = user

  mail(to: @user.email, subject: 'Welcome to My Awesome Site')
end
```

### 发送邮件

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

除了以上方法外，用得比较多的方法还有：

| 方法| 解释 |
|-----|------|
|attachments() | 允许你添加附件到邮件|
|headers(args = nil) | 定制邮件头部|

**创建邮件对象**：细心的你应该发现，我们在 Mailer 类里定义的是实例方法，但创建 mailer 对象用的却是类方法。这里隐藏着魔法，当找不到此类方法时，就会调用 Rails 里重新定义的 method_missing, 找不到方法时先检查方法名是否和 action_methods 一样，如果一样则(把此方法当做参数对待)创建 Mailer 对象。

## 辅助方法

在相应的视图里，用得比较多的辅助方法：

|方法|解释|
|--|--|
|mailer()|当前 Mailer 对象，类似 Controller 对象|
|message()|邮件|
|attachments()|邮件附件|
|format_paragraph(text, len = 72, indent = 2)|处理一段文本消息，行首空两格，每行长度不超过 72 个字符|
|block_format(text)|使用 format_paragraph 处理大段的文本|

## 邮件测试

默认 Rails 提供两个 helper 方法用于测试：

|方法|解释|
|--|--|
|assert_emails(number) | 断言已经发送的邮件数|
|assert_no_emails(&block) | 断言没有邮件发送出去(可用 assert_emails 0 代替)|

assert_emails 和 assert_no_emails 两者本质都是封装 assert_equal.

## 邮件接收

Rails 处理邮件，不常用，而且会比较耗费资源，所以不推荐。但如果你要用的话，需要实现 `receive(raw_mail)` 方法。以接收到的邮件对象，作为唯一的参数。
