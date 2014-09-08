# README 的 补充

## 提示信息

ApplicationController  
这里指的是 Rails 自带的 Rails::ApplicationController 不是我们项目里定义的。
它是下面几个 Controller 的父类，它和 Application 没有对应关系。

WelcomeController  
默认首页 index

MailersController  
邮件预览的 index 和 preview 页面

InfoController
项目信息页面，包括：index、properties、routes

## 命令行(rake & rails)

### tasks.rb 及 tasks 目录

都有单独的 rake 文件，`rake -T` 包含但不限于这下面的命令：

Load Rails Rakefile extensions  
annotations  
documentation  
framework  
log  
middleware  
misc  
routes  
tmp  
engine  
rake db xxx  
**statistics**  
- CodeStatistics
- CodeStatisticsCalculator

### SourceAnnotationExtractor & Annotation

rake notes  
rake notes:optimize

### api

rake rdoc

### test_unit

rake test

### commands

application  
console  
dbconsole  
destroy  
generate  
plugin  
runner  
server  
update

**commands_tasks**

The most common rails commands are:  
  - generate
  - console
  - server
  - dbconsole
  - new

In addition to those, there are:
  - destroy
  - plugin new
  - runner


## BacktraceCleaner

是运用，而不是定义，定义于 ActiveSupport::BacktraceCleaner.

使用类似：

```ruby
bc = BacktraceCleaner.new

bc.add_filter   { |line| line.gsub(Rails.root.to_s, '') } # strip the Rails.root prefix
bc.add_silencer { |line| line =~ /mongrel|rubygems/ } # skip any lines from mongrel or rubygems

bc.clean(exception.backtrace) # perform the cleanup
```

Rails 里用 `Rails.backtrace_cleane` 替换上面的 `bc`.

## Rails Root & Path

```
add, all_paths, autoload_once, autoload_paths
eager_load
load_paths
```
