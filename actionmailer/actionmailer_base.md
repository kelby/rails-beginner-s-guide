## Base

它是我们自定义 Mailer 的父类，是连接我们应用与 Action Mailer 的纽带，是 Action Mailer 连接 Abstract Controller 的纽带。

它是 Action Mailer 里其它模块的集合中心（其它模块不直接对外提供接口，而是通过它完成）。同时，它还提供一些对外的接口，供我们直接使用。

### 作用

它继承于 AbstractController::Base，包含了一些自身及 Abstract Controller 的模块(尽管有的模块它并没有使用到)，作用是为了让它的子类(我们自定义的 Mailer 类)能够"更好用、更实用"。

```
     YourMailer
         |
         V
  ActionMailer::Base
         |
         V
AbstractController::Base
```

### 包含内容

包含了一些默认配置 default_params.

注册拦截器(interceptor)或观察者(observer).

请求到邮件处理 process.

创建邮件实例
(因为 Base include 了多个模块，所以这个实例可以使用这些模块所定义的方法)。

### 邮件(mail)、附件(attachments)

`mail(headers = {}, &block)` 表示邮件对象。<br>
可接收一个代码块做为参数，header(头部)可接受：

| 参数 | 含义 |
| :-- | -- |
| :subject | 主题 |
| :from | 发件人 |
| :to | 收件人 |
| :cc | 抄送 |
| :bcc | 密送 |
| :reply_to | 回邮地址 |
| :date |时间|

> Note: 想了解更多 header 信息，可以点击 [Email#Header_fields](http://en.wikipedia.org/wiki/Email#Header_fields)

使用举例：

```ruby
def welcome(user)
  @user = user

  mail(to: @user.email, subject: 'Welcome to My Awesome Site')
end
```

`attachments()` 允许你在邮件里添加附件。

类型：

```ruby
a_mailer.attachments.class
=> Mail::AttachmentsList

# 类似数组
a_mailer.attachments
=> []
```

使用举例：

```ruby
# 文件名的方式
mail.attachments['filename.jpg'] # => Mail::Part 实例对象或 nil

# 索引的方式
mail.attachments[0]              # => Mail::Part (第一个实例对象)
```

只需要指定文件名、文件类型，然后 Rails 会自动帮你计算出 Content-Type, Content-Disposition, Content-Transfer-Encoding 和 base64 编码等附件内容。

你也可以重写它们：

```ruby
mail.attachments['filename.jpg'] = { mime_type: 'application/x-gzip',
                                     content: File.read('/path/to/filename.jpg') }
```

### 设置默认值(default & default_options=)

`default(value = nil)` 是ActionMailer::Base提供的方法，用来设置 default_params，默认已经有：

```ruby
default_params = {
  mime_version: "1.0",
  charset:      "UTF-8",
  content_type: "text/plain",
  parts_order:  [ "text/plain", "text/enriched", "text/html" ]
}
```

我们可以配置(value 必需是 Hash 类型)：

```ruby
# 下面两者一样
config.action_mailer.default { from: "no-reply@example.org" }
config.action_mailer.default_options = { from: "no-reply@example.org" }

# 常见配置(开发模式下)
config.action_mailer.default_url_options = { :host => 'localhost:3000' }

# 配置邮件发送方式
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

# 邮件发送报错时(比如：邮箱错了)，是否抛异常。开发环境下，可设置为 true
config.action_mailer.raise_delivery_errors = true

# 是否真的发邮件，默认已经是 true
config.action_mailer.perform_deliveries = true
```

它和配置文件里的 `default_options=` 是一样。

在 config/environments/ 目录下针对不同执行环境会有不同的邮件服务器设置：

```ruby
config.action_mailer.default_options = { from: "no-reply@example.org" }
```

前者对当前 Controller 及其子类有效，而后者对当前环境下所有 Controller 有效。除了使用的地方不同，导致作用域稍有不同外，两者本质是一样的。

### 设置头部消息 headers

使用 `headers` 可以设置邮件的头部消息，例如：

```ruby
headers['X-Special-Domain-Specific-Header'] = "SecretValue"

headers 'X-Special-Domain-Specific-Header' => "SecretValue",
        'In-Reply-To' => incoming.message_id
```

直接调用了 Mail::Message#headers 方法，默认已经有选项：

```
:subject
:sender
:from
:to
:cc
:bcc
:reply-to
:orig-date
:message-id
:references
```

### 接收邮件 receive

Rails 处理邮件，不常用，而且会比较耗费资源，所以不推荐。但如果你要用的话，你可以实现 `receive(raw_mail)` 方，唯一的参数就是，接收到的邮件内容。<br>
Rails 会先创建对应的 Mail 邮件对象，之后才进行后续处理。

### 创建邮件对象时的魔法

细心的你应该发现，我们在 Mailer 类里定义的是实例方法，但创建 mailer 对象用的却是类方法。

这里隐藏着魔法，当找不到此类方法时，就会调用 Rails 重新实现的 `method_missing` 类方法, 会先检查 action_methods 里是否有同名方法，如果有，则(把此方法当做参数对待)创建 MessageDelivery 对象。

### 其它方法

类方法：

```ruby
register_interceptor  # 简单封装 Mail.register_interceptor
register_interceptors # 简单封装上面的 register_interceptor, 可注册多个拦截器。

register_observer     # 简单封装 Mail.register_observer
register_observers    # 简单封装上面的 register_observer, 可注册多个观察者。
```

方法：

```
mailer_name & controller_path
```

返回 Mailer 类的名字，默认与类名同名。
