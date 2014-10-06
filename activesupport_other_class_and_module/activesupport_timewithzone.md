## TimeWithZone

类似 Ruby 内置的 [Time](http://ruby-doc.org/core-2.1.0/Time.html)，但 Ruby 内置的 Time 所创建的实例对象局限于 UTC 和系统的 ENV['TZ'] 时区。这里的实例没有此局限，你可以用你想用的时区。

你不能直接使用 new 创建 TimeWithZone 实例对象，但可以用 local, parse, at 和 now 创建 `TimeZone` 实例对象，或者 in_time_zone 创建 `Time` 和 `DateTime` 实例对象。

```ruby
Time.zone = 'Eastern Time (US & Canada)'        # => 'Eastern Time (US & Canada)'
Time.zone.local(2007, 2, 10, 15, 30, 45)        # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.parse('2007-02-10 15:30:45')          # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.at(1170361845)                        # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.now                                   # => Sun, 18 May 2008 13:07:55 EDT -04:00
Time.utc(2007, 2, 10, 20, 30, 45).in_time_zone  # => Sat, 10 Feb 2007 15:30:45 EST -05:00
```

查询 Time 和 TimeZone 的 API 可以对这些方法有更多的了解。

TimeWithZone 创建的实例对象和 Ruby 内置的 Time 创建的实例对象完全兼容，也就是说它们是等价关系。

```ruby
t = Time.zone.now                     # => Sun, 18 May 2008 13:27:25 EDT -04:00
t.hour                                # => 13
t.dst?                                # => true
t.utc_offset                          # => -14400
t.zone                                # => "EDT"
t.to_s(:rfc822)                       # => "Sun, 18 May 2008 13:27:25 -0400"
t + 1.day                             # => Mon, 19 May 2008 13:27:25 EDT -04:00
t.beginning_of_year                   # => Tue, 01 Jan 2008 00:00:00 EST -05:00
t > Time.utc(1999)                    # => true

# 完全兼容，它们是等价关系。
t.is_a?(Time)                         # => true
t.is_a?(ActiveSupport::TimeWithZone)  # => true
```

相关：Date, Time, DateTime, DateAndTime 以及 TimeZone



