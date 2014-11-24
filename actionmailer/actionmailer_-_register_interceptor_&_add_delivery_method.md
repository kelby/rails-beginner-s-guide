## 邮件拦截器及配置发送

为了"开发和测试尽量的接近生产环境"和"知道发送出去的邮件真正的样子"等目的，我们希望在非生产环境下，能够查看邮件发送情况。

## 解决方案

- (开发、测试)用邮件预览 gem

我们可以使用 MailCatcher 或 Letter Opener 等 gem 实现。

- (生产)只发送到特定的邮箱

比如，我们注册一些特殊账号或邮箱，然后发送给我们自己。

- 用拦截器

也就是下面要介绍的。

```ruby
# 注册拦截器(可指定条件)
ActionMailer::Base.register_interceptor(DevelopmentMailInterceptor) if Rails.env.development?

# 注册拦截器(有时候需要先加载)
require 'whitelist_interceptor'
ActionMailer::Base.register_interceptor(WhitelistInterceptor)

# 注册拦截器(有时候需要先加载)
require 'environment_interceptor'
ActionMailer::Base.register_interceptor(EnvironmentInterceptor)
```

```ruby
class DevelopmentMailInterceptor
  # 实现 delivering_email 方法
  # 在最后时刻替换掉发送到的邮箱
  def self.delivering_email message
    message.subject = "[#{message.to}] #{message.subject}"
    message.to = "eifion@asciicasts.com"
  end
end

class WhitelistInterceptor
  # 实现 delivering_email 方法
  # 白名单
  def self.delivering_email message
    unless message.to.join(' ') =~ /(@yourcompany.com|@thoughtworks.com)/i
      message.subject = "#{message.to} #{message.subject}"
      message.to = ENV['NOTIFICATIONS_EMAIL']
    end
  end
end

class EnvironmentInterceptor
  # 实现 delivering_email 方法
  # 有多个非生产环境，但它们都要发送邮件
  def self.delivering_email message
    message.subject = "[#{Rails.env.capitalize}] #{message.subject}" unless Rails.env.production?
  end
end
```

## 另外一种类似功能可以是运用 deliver!

``` ruby
# 配置
config.action_mailer.delivery_method = CustomDelivery

# 对应着的类
class CustomDelivery
  # 重新实现 deliver! 方法
  def deliver!(mail)
    puts mail.to       # receiver@test.com
    puts mail.from     # sender@test.com
    ... ...
  end
end
```

当然，比较实际的做法是"继承"于已有的实现，添加或改变我们要做的功能。

``` ruby
require 'mail'

class CustomSmtpDelivery < ::Mail::SMTP
  # SMTP 配置 (当然，也可以把它们放到配置文件里)
  def initialize(values)
    self.settings = {:address => "smtp.gmail.com",
                     :port => 587,
                     :domain => 'yourdomain',
                     :user_name => "username",
                     :password => "password",
                     :authentication => 'plain',
                     :enable_starttls_auto => true,
                     :openssl_verify_mode => nil
                    }.merge!(values)
  end

  attr_accessor :settings

  # 重新实现 deliver! 方法
  def deliver!(mail)
    # Redirect all mail to your inbox
    mail['to'] = "youremail@domain.com"
    mail['bcc'] = []
    mail['cc'] = []
    super(mail)
  end
end
```

如果你使用的是第三方邮件服务，发送邮件的时候通常还要传递额外的信息供第三方验证，此时你可以用 add_delivery_method 实现方式

## 参考

[Abort mail delivery with Rails 3 interceptors](http://thepugautomatic.com/2012/08/abort-mail-delivery-with-rails-3-interceptors/)<br>
[How to implement an email Interceptor for development](http://blog.crowdint.com/2012/02/23/how-to-implement-an-email-interceptor-for-development.html)<br>
[206: ActionMailer in Rails 3](http://cn.asciicasts.com/episodes/206-actionmailer-in-rails3)<br>
[Tips for Implementing Emails in Rails](http://www.jacopretorius.net/2013/11/tips-for-implementing-emails-in-rails.html)

[Deliver Email With Amazon SES In A Rails app](http://robots.thoughtbot.com/deliver-email-with-amazon-ses-in-a-rails-app)<br>
[Custom mail delivery method in Rails 3.x](http://mdushyanth.wordpress.com/2011/08/06/custom-mail-delivery-method-in-rails-3/)

