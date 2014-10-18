# Subscriber

```
add_event_subscriber, attach_to
F
finish
M
method_added
N
new
S
start, subscribers
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
