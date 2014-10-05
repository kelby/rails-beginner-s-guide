## Base

也就是 ActionMailer::Base，我们继承的就是它。

### 邮件、附件

|方法|解释|
|--|--|
| mail(headers = {}, &block) | 表示邮件对象 |
|attachments() | 允许你在邮件里添加附件|

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

使用举例：

```ruby
def welcome(user)
  @user = user

  mail(to: @user.email, subject: 'Welcome to My Awesome Site')
end
```

`attachments()`

类型：

```ruby
a_mailer.attachments.class
=> Mail::AttachmentsList

# 类似数组
a_mailer.attachments
=> []
```

使用举例(定义)：

```ruby
# By Filename
mail.attachments['filename.jpg'] # => Mail::Part object or nil

# or by index
mail.attachments[0]              # => Mail::Part (first attachment)
```

只需要指定文件名、文件类型，然后 Rails 会自动帮你计算出 Content-Type, Content-Disposition, Content-Transfer-Encoding 和 base64 编码等附件内容。

你也可以重写它们：

```ruby
mail.attachments['filename.jpg'] = { mime_type: 'application/x-gzip',
                                     content: File.read('/path/to/filename.jpg') }
```

### 设置默认值

| 方法 | 解释 |
|--|--|
|default(value = nil) & default_options= | 设置默认值，value 必需是 Hash 类型|

```ruby
# 下面两者一样
config.action_mailer.default { from: "no-reply@example.org" }
config.action_mailer.default_options = { from: "no-reply@example.org" }

# 常见配置(开发模式下)
config.action_mailer.default_url_options = { :host => 'localhost:3000' }

# 下面这些配置，在 Mail 里就有了
# 发送方式
config.action_mailer.delivery_method = :smtp
# 根据不同发送方式，做不同配置
config.action_mailer.smtp_settings = {
  address: 'smtp.gmail.com',
  port: 587,
  domain: 'your_domain.com',
  # 完整的邮箱地址
  username: 'your_email@gmail.com',
  password: 'your_gmail_password',
  authentication: 'plain',
  enable_starttls_auto: true
}

# 邮件发送报错时，是否把错误信息发送给用户。开发环境下，可设置为 true
config.action_mailer.raise_delivery_errors = true
# 默认即是 true
config.action_mailer.perform_deliveries = true
```

它和配置文件里的 `default_options=` 是一样。

> Note: delivery_method 常见有 smtp(Mail::SMTP)、file(Mail::FileDelivery)、sendmail(Mail::Sendmail) 和 test(Mail::TestMailer)，也可以自定义。



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

### 接收邮件

| 方法 | 解释 |
| -- | -- |
| receive(raw_mail) | 表示接收邮件 |

`receive(raw_mail)`

Rails 处理邮件，不常用，而且会比较耗费资源，所以不推荐。但如果你要用的话，你可以实现 `receive(raw_mail)` 方，唯一的参数就是，接收到的邮件对象。

Rails 处理邮件，不常用，而且会比较耗费资源，所以不推荐。但如果你要用的话，需要实现 `receive(raw_mail)` 方法。以接收到的邮件对象，作为唯一的参数。

### 其它

`mailer_name()` 返回文件名，或者 anonymous。

和 Mail 有关联？

headers 返回 Mail对象的 headers 或 Mail对象本身
attachments 返回 Mail对象的 attachments
mail(headers = {}, &block) 返回 Mail对象本身

除了以上方法外，还有：

```ruby
default_i18n_subject
headers # 定制邮件头部
register_interceptor, register_interceptors
register_observer, register_observers
set_content_type
supports_path?
```

[如何使用 Mail](https://github.com/mikel/mail#usage)
