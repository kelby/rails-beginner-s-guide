## Subscriber

```
add_event_subscriber
attach_to
finish
method_added
start
subscribers
```

使用举例：

```
module ActiveRecord
  class StatsSubscriber < ActiveSupport::Subscriber
    def sql(event)
      Statsd.timing("sql.#{event.payload[:name]}", event.duration)
    end
  end
end

ActiveRecord::StatsSubscriber.attach_to :active_record
```
