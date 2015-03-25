### 拦截器 register_interceptor

为了"开发和测试尽量的接近生产环境"和"知道发送出去的邮件真正的样子"等目的，我们希望在非生产环境下，能够查看邮件发送情况。

**解决方案**

- (开发、测试)用邮件预览 gem

我们可以使用 mailcatcher 或 letter_opener 等 gem 实现。

- (生产)只发送到特定的邮箱

比如，我们注册一些特殊账号或邮箱，然后发送给我们自己。

- 用拦截器

也就是下面要介绍的。

注册拦截器一(可指定条件)：

```ruby
if Rails.env.development?
  ActionMailer::Base.register_interceptor(DevelopmentMailInterceptor)
end
```

注册拦截器二(没有加载的话，需要先加载)：

```ruby
require 'whitelist_interceptor'
ActionMailer::Base.register_interceptor(WhitelistInterceptor)
```

注册拦截器三(没有加载的话，需要先加载)：

```ruby
require 'environment_interceptor'
ActionMailer::Base.register_interceptor(EnvironmentInterceptor)
```

上述代码，一般放在 config/application.rb
或 config/environments/file_name.rb
或 config/initializers/file_name.rb
文件里。

实现 delivering_email 方法一，
在最后时刻替换掉要发送到的邮箱。

```ruby
class DevelopmentMailInterceptor
  def self.delivering_email message
    message.subject = "[#{message.to}] #{message.subject}"
    message.to = "overwrite_to@example.com"
  end
end
```

实现 delivering_email 方法二，白名单，群发公司邮箱。

```ruby
class WhitelistInterceptor
  def self.delivering_email message
    unless message.to.join(' ') =~ /(@yourcompany.com)/i
      message.subject = "#{message.to} #{message.subject}"
      message.to = ENV['NOTIFICATIONS_EMAIL']
    end
  end
end
```

实现 delivering_email 方法三，非生产环境时，标明发邮件所在的运行环境。

```ruby
class EnvironmentInterceptor
  def self.delivering_email message
    unless Rails.env.production?
      message.subject = "[#{Rails.env.capitalize}] #{message.subject}"
    end
  end
end
```

上述代码，一般放在 lib/file_name.rb 文件里。

上面大概思路已经给出，实际项目如何使用可以自行决定。
