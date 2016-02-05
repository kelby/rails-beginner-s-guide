## Benchmarkable

**耗时统计**

执行某程序，用了多少时间？

```
benchmark
```

使用举例：

```ruby
<% benchmark 'Process data files', level: :info, silence: true do %>
  <%= expensive_and_chatty_files_operation %>
<% end %>
```

可先用 gem 'newrelic' 等大致定位，然后使用 benchmark 实现精确定位。

链接 [标准库 Benchmark](http://ruby-doc.org/stdlib-2.2.0/libdoc/benchmark/rdoc/Benchmark.html)
