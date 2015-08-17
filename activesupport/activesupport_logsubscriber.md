## Log Subscriber

继承于 Subscriber.

```
Rails 各个模块里的 LogSubscriber
         |
         V
    LogSubscriber
         |
         V
     Subscriber
```

类方法：

```
flush_all!

log_subscribers

logger
```

实例方法：

```
finish
start

logger
```

其它实例方法：

```
info
debug
warn
error
fatal
unknown
```


和

```
color
```

使用举例：

```ruby
module ActiveRecord
  class LogSubscriber < ActiveSupport::LogSubscriber
    def sql(event)
      "#{event.payload[:name]} (#{event.duration}) #{event.payload[:sql]}"
    end
  end
end

ActiveRecord::LogSubscriber.attach_to :active_record
```
