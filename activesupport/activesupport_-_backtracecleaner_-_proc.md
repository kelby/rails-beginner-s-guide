## Backtrace Cleaner

报错或程序运行反馈信息过多、过杂，找不到关键点？你可以使用 Backtrace Cleaner 过滤无用信息。

### 单独使用

```ruby
bc = BacktraceCleaner.new

# 指定过滤某字符串信息
bc.add_filter   { |line| line.gsub(Rails.root, '') }

# 指定过滤某 gem 信息，尽管我们使用了
bc.add_silencer { |line| line =~ /mongrel|rubygems/ }

# 执行过滤。
# clean 与 filter 方法作用完全一样，参数是要处理的信息
bc.clean(exception.backtrace) # perform the cleanup
```

除以上例子使用到的方法外，还有：

| 方法 | 参数 |
|--|--|
| filter & clean | 参数是要处理的信息 |
| remove_filters! | 移除之前的过滤规则 |
| remove_silencers! | 移除之前的过滤规则 | 

### 结合 Rails 使用

可以在以下配置文件，设置过虑条件：

```
config/initializers/backtrace_silencers.rb
```

Rails 启动时就已使用 backtrace_cleaner.

通过以下方式，可以查看：

```ruby
Rails.backtrace_cleaner
=> #<Rails::BacktraceCleaner:0x007ff859bed328
 @filters=
  [#<Proc:0x007ff859bed238@/Users/.../lib/rails/backtrace_cleaner.rb:10>,
   #<Proc:0x007ff859bed210@/Users/.../lib/rails/backtrace_cleaner.rb:11>,
   #<Proc:0x007ff859bed1c0@/Users/.../lib/rails/backtrace_cleaner.rb:12>,
   #<Proc:0x007ff859bec450@/Users/.../lib/rails/backtrace_cleaner.rb:26>],
 @silencers=
  [#<Proc:0x007ff859bec428@/Users/.../lib/rails/backtrace_cleaner.rb:15>]>
```

