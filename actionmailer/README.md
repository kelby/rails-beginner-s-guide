ActionMailer 是 Rails 内建的组件，用来处理邮件相关业务，并且简单好用。

它依赖于 Rails 内建的其它组件 ActiveJob、ActionPack(主要用到的是 AbstractController)和 ActionView 以及外部 gem 'mail'.

因为是 Rails 内建的组件，所以使用上通常集成于 Rails 项目，但其实它也可以在 Rails 之外使用。

## 核心是 gem 'mail'

既然是用来处理邮件相关业务，那么核心功能当然就是邮件处理，而这部分由 gem 'mail' 完成。

gem 'mail' 可用于邮件处理，包括创建、发送、和接收等。mail 可以直接拿来用:

```ruby
require 'mail'

mail = Mail.new do
  from     'from@example.com'
  to       'to@example.com'
  subject  'Here is the image you wanted'
  body     File.read('body.txt')
  add_file :filename => 'somefile.png', :content => File.read('/somefile.png')
end

mail.deliver!
```

当然，上面的邮件并不能发送成功，因为有一些必须的配置还没有写，并且我们也没有 'body.txt' 文件和 '/somefile.png' 图片。更多示例，可以参考 [mail#usage](https://github.com/mikel/mail#usage)

> Note: 单独发送邮件，还可以使用标准库 [Net::SMTP](http://ruby-doc.org/stdlib-2.1.2/libdoc/net/smtp/rdoc/Net/SMTP.html)

## 引入其它，为了更好用

直接使用 gem 'mail'，基本功能是满足了，但并不好用。缺少灵活的配置，内容与模板没有分离等。ActionMailer 改善了它们，举例单独使用 action_mailer：

```ruby
# mailer.rb
require 'action_mailer'

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

    mail(to: "myemail@gmail.com", from: "test@example.com", subject: "testing mail") do |format|
      format.text
      format.html
    end
  end
end

email = Mailer.daily_email
puts email
email.deliver
```

```ruby
# mailer/daily_email.html.erb
<p>this is an html email</p>
<p> and this is a variable <%= @var %> </p>
```

```ruby
# mailer/daily_email.text.erb
this is a text email
and this is a variable <%= @var %>
```

配置部分可以抽取出来，模板和内容可以分开管理，创建和发送邮件也更加直观。

> Note: 以上代码参考了：[ActionMailer 3 without Rails](http://stackoverflow.com/questions/4951310/actionmailer-3-without-rails)

## 引入其它，为了更实用

通常，除了邮件发送外，我们还需要其它功能，这就有一个"集成"的过程。我们希望能够测试、能够自动化、邮件预览等。我们想用 Rails 其它组件提供的 render, before_action, url_for 以及丰富的 helper 方法。

运行下面命令即可自动生成邮件模板：

`rails generate mailer Notifier welcome`

## 实现方式：汝果欲学诗, 功夫在诗外

ActionMailer 本身并没有"实现"什么功能。最基本，也是最核心的部分由 gem 'mail' 完成；为了更好用、更实用，加载或封装了 ActiveJob、ActionPack 和 ActionView。

为了做好、做得上层次，下面是在邮件处理外，ActionMailer "实现"的一些功能：

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
