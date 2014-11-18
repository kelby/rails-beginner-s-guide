# MiniTest Others

## Kernel

很常用的方法

```
describe
```

## Object

常用的方法

```
stub
```

保存调用此方法的值，下次再调用此方法，直接使用该值，不用重新计算生成。

举例


```ruby
DateTime.now
# => 2014-11-17T22:16:15+08:00

DateTime.stub :now, (Date.today.to_date - 14) do
  puts DateTime.now
  # ...
  puts DateTime.now
end

# => 2014-11-03
# => 2014-11-03
```

## AbstractReporter

实例方法

```
passed?
record
report
start
```

## Assertion

实例方法

```
location
```

## BenchSpec

类方法

```
bench
bench_performance_constant
bench_performance_exponential
bench_performance_linear
bench_range
```

## Benchmark

这里的文档说看 Assertions 的文档即可。

类方法

```
bench_exp
bench_linear
bench_range
```

实例方法

```
assert_performance
assert_performance_constant
assert_performance_exponential
assert_performance_linear
assert_performance_logarithmic
assert_performance_power

fit_error
fit_exponential
fit_linear
fit_logarithmic
fit_power

sigma

validation_for_fit
```



