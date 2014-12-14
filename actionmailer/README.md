## Action Mailer

Action Mailer 是 Rails 内建的组件，用来处理邮件相关业务。

它依赖于 Rails 内建的其它组件，如：Active Job、Abstract Controller 和 Action View，以及外部 gem 'mail'.

因为是 Rails 内建的组件，所以使用上通常集成于 Rails 项目，但其实它也可以在 Rails 之外使用。

### 核心是 gem 'mail'

既然是用来处理邮件相关业务，那么核心功能当然就是邮件处理，而这部分由 gem 'mail' 完成。

gem 'mail' 可用于邮件处理，包括创建、发送、和接收等。mail 可以直接拿来用:

```ruby
require 'mail'

Mail.defaults do
  delivery_method :smtp,
    :address              => "smtp.gmail.com",
    :port                 => 587,
    :domain               => "example.com",
    :authentication       => :plain,
    :user_name            => "user_name@gmail.com",
    :password             => "gmail_password",
    :enable_starttls_auto => true
end

mail = Mail.new do
  from     'no-reply@example.com'
  to       'hello@world.com'
  subject  'First multipart email sent with Mail'

  text_part do
    body 'This is plain text'
  end

  html_part do
    content_type 'text/html; charset=UTF-8'
    body '<h1>This is HTML</h1>'
  end
end

mail.deliver!
```

上面的例子使用了 Gmail 做为邮件服务器，所以需要用到 Gmail 用户名和密码，但实际上你也可以在本地搭建或使用其它第三方邮件服务器。更多示例，可以参考 [mail#usage](https://github.com/mikel/mail#usage)

> Note: 单独发送邮件，还可以使用标准库 [Net::SMTP](http://ruby-doc.org/stdlib-2.1.2/libdoc/net/smtp/rdoc/Net/SMTP.html)

### 引入其它，为了更好用

直接使用 gem 'mail'，创建、发送邮件等最最基本的功能是实现了，但并不好用。缺少灵活的配置，内容与模板没有分离等。Action Mailer 改善了它们，举例单独使用 action_mailer：

```ruby
# mailer.rb
require 'action_mailer'

# 邮件发送报错时，是否把错误信息发送给用户。开发环境下，可设置为 true
ActionMailer::Base.raise_delivery_errors = true

ActionMailer::Base.delivery_method = :smtp
ActionMailer::Base.smtp_settings = {
  :address => "smtp.gmail.com",
  :port    => 587,
  :domain  => "example.com",
  :authentication => :plain,
  :user_name      => "test@example.com",
  :password       => "passw0rd",
  :enable_starttls_auto => true
}

ActionMailer::Base.view_paths = File.dirname(__FILE__)

class Mailer < ActionMailer::Base
  def daily_email
    @var = "var"

    # 发件人是这里的 from，不是上面 smtp_settings 里设置的任何一个
    mail(to: "to@gmail.com", from: "test@example.com", subject: "test") do |format|
      format.text
      format.html
    end
  end
end

email = Mailer.daily_email
email.deliver_now
```

```ruby
# mailer/daily_email.html.erb
<p>this is an html email</p>
<p> and this is a variable <%= @var %></p>
```

```ruby
# mailer/daily_email.text.erb
this is a text email
and this is a variable <%= @var %>
```

配置部分可以抽取出来，模板和内容可以分开管理，创建和发送邮件也更加直观。

### 引入其它，为了更实用

通常，除了邮件发送外，我们还需要其它功能，这就有一个"集成"的过程。我们希望能够测试、能够自动化、邮件预览等。我们想用 Rails 其它组件提供的 render, before_action, url_for 以及丰富的 helper 方法。

运行下面命令即可自动生成邮件模板：

`rails generate mailer Notifier welcome`

### 实现方式：汝果欲学诗, 功夫在诗外

Action Mailer 本身并没有"实现"什么功能。最基本，也是最核心的部分由 gem 'mail' 完成；为了更好用、更实用，加载或封装了 Active Job、Action Pack 和 Action View.

为了做好、做得上层次，下面是在邮件处理外，Action Mailer "实现"的一些功能：

- 测试
- 日志记录
- 邮件预览
- 延迟发送
- 灵活的配置
- 内容与模板分离
- 丰富实用的 helper 方法
- 自动化(如：rails g mailer)
- 集成(有时我们要的不仅仅只是邮件服务)
- 拦截器、观察者(发送之前、之后想做点什么？)
