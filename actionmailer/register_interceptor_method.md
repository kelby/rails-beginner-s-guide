## 邮件拦截器及配置发送

为了"开发和测试尽量的接近生产环境"和"知道发送出去的邮件真正的样子"等目的，我们希望在非生产环境下，能够查看邮件发送情况。

### 解决方案

- (开发、测试)用邮件预览 gem

我们可以使用 mailcatcher 或 letter_opener 等 gem 实现。

- (生产)只发送到特定的邮箱

比如，我们注册一些特殊账号或邮箱，然后发送给我们自己。

- 用拦截器

也就是下面要介绍的。

```ruby
# 注册拦截器(可指定条件)
if Rails.env.development?
  ActionMailer::Base.register_interceptor(DevelopmentMailInterceptor)
end

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
    unless message.to.join(' ') =~ /(@yourcompany.com)/i
      message.subject = "#{message.to} #{message.subject}"
      message.to = ENV['NOTIFICATIONS_EMAIL']
    end
  end
end

class EnvironmentInterceptor
  # 实现 delivering_email 方法
  # 有多个非生产环境，但它们都要发送邮件
  def self.delivering_email message
    unless Rails.env.production?
      message.subject = "[#{Rails.env.capitalize}] #{message.subject}"
    end
  end
end
```

参考

[Abort mail delivery with Rails 3 interceptors](http://thepugautomatic.com/2012/08/abort-mail-delivery-with-rails-3-interceptors/)<br>
[How to implement an email Interceptor for development](http://blog.crowdint.com/2012/02/23/how-to-implement-an-email-interceptor-for-development.html)<br>
[206: ActionMailer in Rails 3](http://cn.asciicasts.com/episodes/206-actionmailer-in-rails3)<br>
[Tips for Implementing Emails in Rails](http://www.jacopretorius.net/2013/11/tips-for-implementing-emails-in-rails.html)
