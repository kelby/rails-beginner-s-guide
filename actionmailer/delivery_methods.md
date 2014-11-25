## Delivery Methods 定制与新增

为了"开发和测试尽量的接近生产环境"和"知道发送出去的邮件真正的样子"等目的，我们希望在非生产环境下，能够查看邮件发送情况。

delivery_method 默认有 smtp(Mail::SMTP)、file(Mail::FileDelivery)、sendmail(Mail::Sendmail) 和 test(Mail::TestMailer)，也可以自定义。


## 解决方案

- (开发、测试)用邮件预览 gem

我们可以使用 MailCatcher 或 Letter Opener 等 gem 实现。

- (生产)只发送到特定的邮箱

比如，我们注册一些特殊账号或邮箱，然后发送给我们自己。

## 重新实现，并添加 delivery_method

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


原理

```ruby
def add_delivery_method(symbol, klass, default_options={})
  class_attribute(:"#{symbol}_settings") unless respond_to?(:"#{symbol}_settings")
  send(:"#{symbol}_settings=", default_options)
  self.delivery_methods = delivery_methods.merge(symbol.to_sym => klass).freeze
end
```

使用

```ruby
add_delivery_method :sendmail, Mail::Sendmail,
  location:  '/usr/sbin/sendmail',
  arguments: '-i -t'
```

如何运行

```ruby
klass = delivery_methods[method]

mail.delivery_method(klass, (send(:"#{method}_settings") || {}).merge(options || {}))
````

## 参考

[Deliver Email With Amazon SES In A Rails app](http://robots.thoughtbot.com/deliver-email-with-amazon-ses-in-a-rails-app)<br>
[Custom mail delivery method in Rails 3.x](http://mdushyanth.wordpress.com/2011/08/06/custom-mail-delivery-method-in-rails-3/)
