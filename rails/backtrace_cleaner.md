## Backtrace Cleaner

是运用，而不是定义，定义于 ActiveSupport::BacktraceCleaner.

使用类似：

```ruby
bc = BacktraceCleaner.new

# 定义：(部分)删除内容里的 Rails.root
bc.add_filter   { |line| line.gsub(Rails.root.to_s, '') }

# 定义：(整行)删除 mongrel 或 rubygems 里记录的内容
bc.add_silencer { |line| line =~ /mongrel|rubygems/ }

# 执行
bc.clean(exception.backtrace)
```

Rails 里用 `Rails.backtrace_cleane` 替换上面的 `bc`.
