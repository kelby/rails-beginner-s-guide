# ActiveSupport - BacktraceCleaner - proc 的运用
## BacktraceCleaner
-----------------

管理跟踪系统。

-----------

Backtraces often include many lines that are not relevant for the context under review. This makes it hard to find the signal amongst the backtrace noise, and adds debugging time. With a BacktraceCleaner, filters and silencers are used to remove the noisy lines, so that only the most relevant lines remain.

Filters are used to modify lines of data, while silencers are used to remove lines entirely. The typical filter use case is to remove lengthy path information from the start of each line, and view file paths relevant to the app directory instead of the file system root. The typical silencer use case is to exclude the output of a noisy library from the backtrace, so that you can focus on the rest.

```ruby
bc = BacktraceCleaner.new

# 对 backtrace 结果进行过滤，显示效果更友好
bc.add_filter   { |line| line.gsub(Rails.root, '') } # strip the Rails.root prefix

# 不想包含在 backtrace 里，尽管我们使用了
bc.add_silencer { |line| line =~ /mongrel|rubygems/ } # skip any lines from mongrel or rubygems

bc.clean(exception.backtrace) # perform the cleanup
```

`add_filter(&block)`

设置 filter，它是一个 proc，需要一个参数

```
# 函数.对象
filter.call(line)

# 对象.方法
line.filter
```

To reconfigure an existing BacktraceCleaner (like the default one in Rails) and show as much data as possible, you can always call BacktraceCleaner#remove_silencers!, which will restore the backtrace to a pristine state. If you need to reconfigure an existing BacktraceCleaner so that it does not filter or modify the paths of any lines of the backtrace, you can call BacktraceCleaner#remove_filters! These two methods will give you a completely untouched backtrace.

Inspired by the Quiet Backtrace gem by Thoughtbot.

--------

Rails 配置文件

`config/initializers/backtrace_silencers.rb`

并且，Rails 启动时就启动它：

```ruby
def backtrace_cleaner
  @backtrace_cleaner ||= begin
    # Relies on Active Support, so we have to lazy load to postpone definition until AS has been loaded
    require 'rails/backtrace_cleaner'
    # initialize 时就已经有行为了
    Rails::BacktraceCleaner.new
  end
end
```

```
pry(main)> Rails.backtrace_cleaner
=> #<Rails::BacktraceCleaner:0x007ff859bed328
 @filters=
  [#<Proc:0x007ff859bed238@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/railties-3.2.17/lib/rails/backtrace_cleaner.rb:10>,
   #<Proc:0x007ff859bed210@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/railties-3.2.17/lib/rails/backtrace_cleaner.rb:11>,
   #<Proc:0x007ff859bed1c0@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/railties-3.2.17/lib/rails/backtrace_cleaner.rb:12>,
   #<Proc:0x007ff859bec450@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/railties-3.2.17/lib/rails/backtrace_cleaner.rb:26>],
 @silencers=
  [#<Proc:0x007ff859bec428@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/railties-3.2.17/lib/rails/backtrace_cleaner.rb:15>]>
```

使用举例：`rails/backtrace_cleaner.rb`

