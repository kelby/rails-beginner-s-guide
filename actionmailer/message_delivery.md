## Message Delivery

专用于邮件的发送。

这个文件是重构 + 引入 DeliveryJob 后产生的。

```
deliver_now
deliver_now!

deliver_later
deliver_later!

message
```

deliver 直接调用了 gem 'mail' 提供的方法，或加入延迟任务再调用。

通常，我们都是创建邮件并发送

```ruby
# Creates the email and sends it immediately
Notifier.welcome("helloworld@example.com").deliver_now
```

也可以先创建邮件对象，稍后邮件对象调用方法发送邮件：

```ruby
message = Notifier.welcome("helloworld@example.com") # => an ActionMailer::MessageDeliver object
message.deliver_now                                  # sends the email
```

再或者，自动延迟发送

```ruby
Notifier.welcome(david).message     # => a Mail::Message object
```

> NOTE: 现在 Rails 默认已经有延迟发送的方法。
