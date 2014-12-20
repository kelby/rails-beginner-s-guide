## Message Delivery

专用于邮件的发送。(YourMailer#action 时已经使用，创建的就是 MessageDelivery 实例对象)

这个文件是重构 + 引入 DeliveryJob 后产生的。

```
deliver_now
deliver_now!

deliver_later
deliver_later!

message
```

`deliver_later` 和 `deliver_later!` 接受参数 :wait、:wait_until 或 :queue.

`message` 表示邮件对象，即 Mail::Message 实例对象。

执行 deliver 操作时，会得先到 message 对象，然后直接调用 gem 'mail' 提供的方法，或加入延迟任务再调用。

通常，我们都是创建邮件并发送

```ruby
Notifier.welcome("helloworld@example.com").deliver_now
```

也可以先创建邮件对象，稍后邮件对象调用方法发送邮件：

```ruby
message = Notifier.welcome("helloworld@example.com")
# => an ActionMailer::MessageDeliver object

message.deliver_now
# sends the email
```

获取 Mail::Message 实例对象(deliver 操作其实是由它发出)

```ruby
Notifier.welcome(david).message
```
