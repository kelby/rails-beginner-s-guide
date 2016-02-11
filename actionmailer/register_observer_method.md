#### 订阅者 register_observer

邮件发送之后想做点什么？-- 使用观察者。

类似 register_interceptor，使用观察者：

```ruby
class MailObserver
  # 实现 delivering_email 方法
  def self.delivered_email message
    # ...
  end
end

ActionMailer::Base.register_observer(MailObserver)
```

注意：这里实现的是 `delivered_email` 方法，而之前实现的是 `delivering_email` 方法。
