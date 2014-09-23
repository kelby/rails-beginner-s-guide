## Base

也就是 ActionMailer::Base，我们继承的就是它。

|方法|解释|
|--|--|
|default(value = nil) | value必需是Hash类型|
|attachments() | 允许你在邮件里添加附件|

`attachments()` 使用举例：

```ruby
mail.attachments['filename.jpg'] = File.read('/path/to/filename.jpg')
```

Rails 会自动获取文件名、文件类型，然后帮你计算出 Content-Type, Content-Disposition, Content-Transfer-Encoding 和 base64编码的附件内容。

你也可以重写它们：

```ruby
mail.attachments['filename.jpg'] = {mime_type: 'application/x-gzip',
                                    content: File.read('/path/to/filename.jpg')}
```

你也要可以指定附件：

```ruby
# By Filename
mail.attachments['filename.jpg']   # => Mail::Part object or nil

# or by index
mail.attachments[0]                # => Mail::Part (first attachment)
```

`default(value = nil)` 使用举例：

```ruby
# 下面两者一样
config.action_mailer.default { from: "no-reply@example.org" }
config.action_mailer.default_options = { from: "no-reply@example.org" }

# 常见配置(开发模式下)
config.action_mailer.default_url_options = { :host => 'localhost:3000' }

# 下面这些配置，在 Mail 里就有了
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  address: 'smtp.gmail.com',
  port: 587,
  domain: 'your_domain.com',
  username: 'your_email@gmail.com',
  password: 'your_password',
  authentication: 'plain',
  enable_starttls_auto: true
}
config.action_mailer.raise_delivery_errors = true
config.action_mailer.perform_deliveries = true
```

设置默认值，和配置文件里的 `default_options=` 一样。

| 方法 | 解释 |
| -- | -- |
| mail(headers = {}, &block) | 表示邮件对象 |
| receive(raw_mail) | 表示接收邮件 |

`mail(headers = {}, &block)` header(头部)可接受以下参数，并且最后可接收一个代码块：

```
:subject - 主题
:to - 收件人
:from - 发件人
:cc - 抄送
:bcc - 密送
:reply_to - 回邮地址
:date - 时间
```

`receive(raw_mail)`

Rails 处理邮件，不常用，而且会比较耗费资源，所以不推荐。但如果你要用的话，你可以实现 `receive(raw_mail)` 方，唯一的参数就是，接收到的邮件对象。

`mailer_name()` 返回文件名，或者anonymous。

和 Mail 有关联？

headers 返回 Mail对象的 headers 或 Mail对象本身
attachments 返回 Mail对象的 attachments
mail(headers = {}, &block) 返回 Mail对象本身

[如何使用 Mail](https://github.com/mikel/mail#usage)

## MailHelper

`attachments()`  
表示邮件内容里的附件。

`mailer()`  
表示邮件所在的Controller对象。

`message()`  
表示邮件本身。

`block_format(text)`  
格式化文本，行首空两格，每行长度不超过 72 个字符。(邮件的标准格式，就是这样的)

`format_paragraph(text, len = 72, indent = 2)`  
格式化文本。*len* 为每行长度，*index* 为行首空格数。

> Note: ActiveMailer 可以单独使用，并不绑定于 Rails<br/>

## Base 方法列表

```ruby
attachments
default, default_i18n_subject
headers
mail, mailer_name
receive, register_interceptor, register_interceptors, register_observer, register_observers
set_content_type, supports_path?
```

有一些，在上面已经介绍过了。

另外，它包含了一些子模块，根据 Ruby 规则，它们也是可调用的。包括：

ActionMailer::DeliveryMethods - 邮件发送
ActionMailer::Previews - 邮件预览
AbstractController::Rendering - 渲染
AbstractController::Helpers - 引进、输出辅助方法
AbstractController::Translation - I18n 相关
AbstractController::Callbacks - 支持回调
ActionView::Layouts - 视图布局等

## MessageDelivery

专用于邮件的发送。

这个文件是重构 + 引入 DeliveryJob 后产生的。

```
deliver_now, deliver_now!
deliver_later, deliver_later!

message
```

## 其它

ActionMailer::Base 继承于类 AbstractController::Base,
又包含，但不限于以下'外部'模块

```
AbstractController::Rendering

AbstractController::Logger
AbstractController::Helpers
AbstractController::Translation
AbstractController::AssetPaths
AbstractController::Callbacks

ActionView::Layouts
```

`Dar < Car < Bar` 

有时候 Car 要 extend 前人 Bar，并不是为了 Car 自己使用，而是为了方便后人 Dar. ActionMailer::Base 部分代码充当的角色和 Car 一样，它自己却没有使用，而是为了方便我们自定义的 Mailer 类调用。

如：

Mailer 里：

```
before_action :add_inline_attachment!

layout "mailer"

helper :application
```

View 里：

```
<%= url_for(host: "example.com", controller: "welcome", action: "greeting") %>

<%= users_url(host: "example.com") %>
```


