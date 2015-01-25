### Benchmark

```
ms
```

使用举例：

```ruby
Benchmark.realtime { User.all }
# => 8.0e-05

# 和 realtime 一样, 但单位是"毫秒"
Benchmark.ms { User.all }
# => 0.074
```
