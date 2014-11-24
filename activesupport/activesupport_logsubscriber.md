## Log Subscriber

继承于 Subscriber

提供方法：

```
color

finish, flush_all!
log_subscribers, logger
start
```

和

```
info
debug
warn
error
fatal
unknown
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
