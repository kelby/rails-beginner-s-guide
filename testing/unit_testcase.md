## TestCase

实例方法：

```
setup
teardown
```

除上述方法外，还有：

```
io
io?

run

passed?
```

类方法：

```
i_suck_and_my_tests_are_order_dependent! # 让测试按照顺序执行！

make_my_diffs_pretty! # 错误日志格式化，更漂亮，但会降慢速度

parallelize_me! # 并行执行测试

benchmark_suites

bench_exp
bench_linear
bench_range
```

链接 [MiniTest::Unit::TestCase](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/minitest/rdoc/MiniTest/Unit/TestCase.html)
