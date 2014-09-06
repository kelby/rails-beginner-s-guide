# README 的 补充

## 提示信息

ApplicationController<br>
这里是 Rails 自身的 ApplicationController 不是我们项目里的那个。
它是下面几个 Controller 的父类，它和 Application 联系不大，不要被名字欺骗了。

但它继承于 ActionController::Base 这个功能可很强大。

WelcomeController<br>
默认首页 index

MailersController<br>
邮件预览的 index 和 preview 页面

InfoController
项目信息页面，包括：index、properties、routes

## 命令行

rake rdoc 生成 API 文档<br>
rake test 测试


**commands**

rails 命令行

**tasks**

rake 命令行


## BacktraceCleaner

是运用，而不是定义，定义于 ActiveSupport::BacktraceCleaner.

- `Rails.backtrace_cleaner`

```ruby
def backtrace_cleaner
  @backtrace_cleaner ||= begin
    # Relies on Active Support, so we have to lazy load to postpone definition until AS has been loaded
    require 'rails/backtrace_cleaner'
    Rails::BacktraceCleaner.new
  end
end
```

- 有哪些方法?

```ruby
bc = BacktraceCleaner.new
bc.add_filter   { |line| line.gsub(Rails.root.to_s, '') } # strip the Rails.root prefix
bc.add_silencer { |line| line =~ /mongrel|rubygems/ } # skip any lines from mongrel or rubygems
bc.clean(exception.backtrace) # perform the cleanup
```


- `config/initializers/backtrace_silencers.rb`

You can add backtrace silencers for libraries that you're using but don't wish to see in your backtraces.

```
Rails.backtrace_cleaner.add_silencer { |line| line =~ /my_noisy_library/ }
```

You can also remove all the silencers if you're trying to debug a problem that might stem from framework code.

```
Rails.backtrace_cleaner.remove_silencers!
```

