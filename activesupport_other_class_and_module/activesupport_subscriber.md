## Subscriber

```
     Subscriber
         |
         V
    LogSubscriber
         |
         V
Rails 各个模块里的 LogSubscriber
```

只需继承于它，并定义同名方法，然后 attach_to 即可。

类方法：

```
attach_to

method_added

subscribers
```

`attach_to` 会把子类里的实例方法让 Notifications 的实例对象进行 subscribe (调用 add_event_subscriber)。

其它类方法：

```
add_event_subscriber
```

`add_event_subscriber` 核心实现。

实例方法：

```
start
finish
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

使用到了 Notifications，也就是说依赖于 Notifications 这里的"订阅"，本质还是 Notifications 的订阅。只是为了方便使用而来的。

> Note: 原 Notifications 的 instrument 不变，但 subscribe 太麻烦了，所以才有 Subscriber 以及它的各个子类。
