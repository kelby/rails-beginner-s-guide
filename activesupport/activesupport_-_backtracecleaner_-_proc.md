## Backtrace Cleaner

报错或程序运行反馈信息过多、过杂，找不到关键点？你可以使用 Backtrace Cleaner 过滤无用信息。

#### 单独使用

三步即可：
```ruby
# 1) 创建对象
bc = BacktraceCleaner.new

# 2) 添加规则。可指定'替换'某字符串信息：
bc.add_filter   { |line| line.gsub(Rails.root, '') }
# 2) 添加规则。可指定'删除'某 gem 信息：
bc.add_silencer { |line| line =~ /mongrel|rubygems/ }

# 3) 执行过滤。从 exception.backtrace 里过滤掉符合规则的字符串
bc.clean(exception.backtrace) # 
```

除以上例子使用到的方法外，还有：

| 方法 | 参数 |
|--|--|
| filter & clean | 执行过滤。参数是要处理的信息 |
| remove_filters! | 移除之前的'替换'规则 |
| remove_silencers! | 移除之前的'删除'规则 | 

#### 结合 Rails 使用

Rails 启动时就已使用 backtrace_cleaner, 并且抛异常时会对异常消息进行过滤。

也就是说，Rails 已经帮我们做了第 1 和第 3 步，我们只要做第 2 步"添加规则"即可。
可以在以下配置文件，设置过滤条件：

```ruby
# config/initializers/backtrace_silencers.rb
Rails.backtrace_cleaner.add_silencer { |line| line =~ /my_noisy_library/ }
```

通过以下方式，可以查看 backtrace_cleaner 的 filters 和 silencers 信息：

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

附(Rails 里的 backtrace_cleaner 的定义与调用)：

**定义：**

```ruby
# railties/lib/rails/backtrace_cleaner.rb
module Rails
  class BacktraceCleaner < ActiveSupport::BacktraceCleaner
    # ...
  end
end
    
# railties/lib/rails.rb
def backtrace_cleaner
  @backtrace_cleaner ||= begin
  require 'rails/backtrace_cleaner'
  Rails::BacktraceCleaner.new
  end
end
    
# railties/lib/application.rb
def env_config
  # ...
  "action_dispatch.backtrace_cleaner" => Rails.backtrace_cleaner
  # ...
end
```

**调用：**

```ruby
# action_dispatch/middleware/exception_wrapper.rb
def backtrace_cleaner
  @backtrace_cleaner ||= @env['action_dispatch.backtrace_cleaner']
end
    
def clean_backtrace(*args)
  if backtrace_cleaner
    backtrace_cleaner.clean(backtrace, *args)
  else
    backtrace
  end
end
```
