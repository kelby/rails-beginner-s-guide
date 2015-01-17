## Duration

和 Date、Time、DateTime、TimeWithZone 等时间相关类的关系比较深。

```
ago & until

from_now & since
```

使用举例：

```ruby
1.month.ago # 等价于 Time.now.advance(months: -1)
```
