## Delivery Methods 定制与新增

**底层如何实现邮件"发送"？**

Rails 本身没有实现此功能，但提供选择：调用 Rails 已有发送程序，使用已有邮件服务，新增。

### 调用 Rails 已有发送程序

delivery_method 默认已经有：

```ruby
:smtp     => Mail::SMTP
:file     => Mail::FileDelivery
:sendmail => Mail::Sendmail
:test     => Mail::TestMailer
```

我们可以根据需求，选择、配置、使用它们。

### 使用已有邮件服务

- 如使用 gem 'letter_opener' 直接配置：

```ruby
# config/environments/development.rb
config.action_mailer.delivery_method = :letter_opener
```

- 如使用 gem 'aws-ses' 配置：

```ruby
ActionMailer::Base.add_delivery_method :ses, AWS::SES::Base,
  :access_key_id     => 'abc',
  :secret_access_key => '123'
```

然后：

```ruby
config.action_mailer.delivery_method = :ses
```

### 新增

完全的"新增"邮件服务，难度上比较大。比较实际的做法是"继承"于已有的实现，并且在 Rails 里注册一下，然后使用。

1) "继承"于已有的实现(这里是 SMTP)：

``` ruby
require 'mail'

class CustomSmtpDelivery < ::Mail::SMTP
  # SMTP 配置
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

  # 或者，把 SMTP 配置放到配置文件里
  def initialize(options = {})
    self.settings = options
  end

  attr_accessor :settings

  # 必须重新实现 deliver! 方法
  def deliver!(mail)
    # 把收箱人替换成指定的，这里功能类似拦截器
    mail['to'] = "to@example.com"
    mail['bcc'] = []
    mail['cc'] = []
    super(mail)
  end
end
```

2) 在 Rails 里注册一下：

```ruby
add_delivery_method :custom_smtp_delivery, CustomSmtpDelivery
```

3) 配置使用刚才新增的 delivery method：

```ruby
# config/environments/development.rb
config.action_mailer.delivery_method = :custom_smtp_delivery
```

在之后配置就变成了：

```ruby
ActionMailer::Base.custom_smtp_delivery_settings = {
  :address => "smtp.gmail.com",
  :port => 587,
  # ...
}
```

Rails 已有发送程序及 gem 'letter_opener' 就是用这种方式定义，之后提供给我们使用的。

### 其它

类方法：

```ruby
# 获取所有可用的 delivery_methods
class_attribute :delivery_methods

module ClassMethods
  # 必须结合 delivery_method :test 使用，存放着已经 deliver 的邮件对象，测试的时候可用到它。
  delegate :deliveries, :deliveries=, to: Mail::TestMailer
end
```
