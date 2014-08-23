# ActionDispatch - Benchmarkable - 标准库的运用
执行某程序，用了多少时间？

```ruby
module Benchmarkable
  # 简单封装和使用标准库
  def benchmark(message = "Benchmarking", options = {})
    # 在日志里打印
    if logger
      options.assert_valid_keys(:level, :silence)
      options[:level] ||= :info

      result = nil
      ms = Benchmark.ms { result = options[:silence] ? silence { yield } : yield }
      logger.send(options[:level], '%s (%.1fms)' % [ message, ms ])
      result
    else
      # 如果没有日志，就什么也不做
      yield
    end
  end
end

# 简单封装和使用标准库
class << Benchmark
  def ms
    1000 * realtime { yield }
  end
end
```
