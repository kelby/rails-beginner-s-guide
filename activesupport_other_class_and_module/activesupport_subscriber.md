## Subscriber

区别于 Notifications: 不用发布(instrument)，直接订阅(subscribe)，完全不影响原有代码。

只需继承于它，并定义同名方法即可。

```
attach_to

start
finish

method_added

subscribers
```

```
add_event_subscriber
```

使用举例：

```ruby
module ActiveRecord
  class StatsSubscriber < ActiveSupport::Subscriber
    def sql(event)
      Statsd.timing("sql.#{event.payload[:name]}", event.duration)
    end
  end
end

ActiveRecord::StatsSubscriber.attach_to :active_record
```
