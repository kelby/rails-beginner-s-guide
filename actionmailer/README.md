ActionMailer 是 Rails 内建的发送邮件的 gem.

它依赖于 Rails 内建的 actionpack(包括abstract_controller, action_controller, action_dispack)和 actionview 以及外部的 mail.

使用上，通常集成于 Rails，但也可以独立于 Rails.

## 核心是 gem 'mail'

核心功能当然是邮件处理，这部分由 gem 'mail' 完成。

gem 'mail' 可用于邮件处理，包括创建、发送、和接收等。gem 本身可以直接使用：

```ruby
require 'mail'

mail = Mail.new do
  from     'me@test.lindsaar.net'
  to       'you@test.lindsaar.net'
  subject  'Here is the image you wanted'
  body     File.read('body.txt')
  add_file :filename => 'somefile.png', :content => File.read('/somefile.png')
end

mail.deliver!
```

当然，上面的邮件并不能发送成功，因为有一些配置我们还没有写，并且我们也没有 'body.txt' 文件和 '/somefile.png' 图片。更多示例，可以参考 [mail#usage](https://github.com/mikel/mail#usage)

## 引入其它，为了更好用

直接使用 gem 'mail'，功能上基本都有了，但并不好用。缺少灵活的配置，内容与模板没有分离等。ActionMailer 改善了它们，举例单独使用 action_mailer：

```ruby
# mailer.rb
require 'action_mailer'

ActionMailer::Base.raise_delivery_errors = true
ActionMailer::Base.delivery_method = :smtp
ActionMailer::Base.smtp_settings = {
  :address => "smtp.gmail.com",
  :port    => 587,
  :domain  => "domain.com.ar",
  :authentication => :plain,
  :user_name      => "test@domain.com.ar",
  :password       => "passw0rd",
  :enable_starttls_auto => true
}

ActionMailer::Base.view_paths = File.dirname(__FILE__)

class Mailer < ActionMailer::Base
  def daily_email
    @var = "var"

    mail(to: "myemail@gmail.com", from: "test@domain.com.ar", subject: "testing mail") do |format|
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

> **Note:** 以上代码参考了：[ActionMailer 3 without Rails](http://stackoverflow.com/questions/4951310/actionmailer-3-without-rails)

## 引入其它，为了更实用

通常，除了邮件发送外，我们还需要其它功能，这就有一个"集成"的过程。我们希望能够测试、能够自动化、邮件预览等。我们想用 Rails 其它组件提供的 render, before_action, url_for 以及丰富的 helper 方法。

运行下面命令即可自动生成邮件模板：

`rails generate mailer Notifier welcome`

## 实现方式：汝果欲学诗, 功夫在诗外

ActionMailer 本身并没有"实现"什么功能。最基本，也是最核心的部分由 gem 'mail' 完成；为了更好用、更实用，加载或封装了 actionpack 和 actionview。

为了做好、做得上层次，下面是在邮件处理外，ActionMailer "实现"的一些功能：

- 测试
- 自动化(rails g mailer 虽然简单，但很贴心)
- 邮件预览
- 丰富实用的 helper 方法
- 日志记录
- 发送之前、之后想做点事？(拦截器、观察者)
- 灵活的配置
- 内容与模板分离
- 集成(有时我们要的不仅仅只是邮件服务)
- 延迟发送
