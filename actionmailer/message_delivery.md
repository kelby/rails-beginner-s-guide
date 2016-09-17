## Message Delivery

**应用层面的邮件发送。**

发送邮件时，会调用 YourMailer#action，在这里就会创建 Message Delivery 实例对象。

实例方法：

```
deliver_now
deliver_now!

deliver_later
deliver_later!
```

和

```
message
```

`deliver_now` 和 `deliver_now!` 立即发送邮件。
<br>
`deliver_later` 和 `deliver_later!` 延迟发送邮件，可接受参数 :wait、:wait_until 或 :queue.

通常，我们都是创建邮件并发送：

```ruby
Notifier.welcome("helloworld@example.com").deliver_now
```

也可以先创建邮件对象，稍后邮件对象调用方法发送邮件：

```ruby
message = Notifier.welcome("helloworld@example.com")
# => 得到 ActionMailer::MessageDeliver 实例对象

message.deliver_now
# 执行发送邮件
```

无论哪种方式发送邮件，都要先得到 message 对象，调用 deliver 方法，然后调用 gem 'mail' 相关代码实现发送。

获取 Mail::Message 实例对象(deliver 操作其实是由它发出)：

```ruby
# 表示邮件对象，它本质是 Mail::Message 的实例对象
message = Notifier.welcome(david).message
```

其它使用示例：

```ruby
Notifier.welcome(User.first).deliver_later
Notifier.welcome(User.first).deliver_later(wait: 1.hour)
Notifier.welcome(User.first).deliver_later(wait_until: 10.hours.from_now)
```

> Note: 这里的邮件"发送"，指的是我们应用层面的"发送"。至于 Rails 底层如何实现邮件"发送"，参考【Delivery Methods】章节。
