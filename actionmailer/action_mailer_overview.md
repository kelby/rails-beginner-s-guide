## Base

也就是 ActionMailer::Base，我们继承的就是它。

```
default(value = nil) - value必需是Hash类型
attachments()
```

`attachments()`

允许你在邮件里添加附件，例如：

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

`default(value = nil)`

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

设置默认值，和 配置文件里的 `default_options=` 一样。

`mail(headers = {}, &block)`

可接收一个代码块做为参数，header(头部)可接受：

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
> Note: C < B < A 有时候之所以 B 要 extend A 并不是为了 B 自己使用，而仅仅是为了方便 C。所以 ActionMailer::Base 才会 include 一堆代码，尽管有的对它本身没有用。
