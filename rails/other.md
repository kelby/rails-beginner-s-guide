## 提示信息

ApplicationController  
这里指的是 Rails 自带的 Rails::ApplicationController, 不是我们应用里定义的。
它是下面几个 Controller 的父类，并且它和 Application 没有对应关系。

WelcomeController  
新建 Rails 项目时，默认首页 index.

MailersController  
邮件预览的 index 和 preview 页面。

InfoController & Info  
项目信息页面，包括：index(同 routes) 和 properties.

## 命令行(rake & rails)

### tasks.rb 及 tasks 目录

都有单独的 rake 文件，`rake -T` 包含但不限于这下面的命令：

annotations(notes)  
documentation(doc)  
framework(rails)  
log  
middleware  
misc(secret、about、time)  
routes  
tmp  
engine(app、db)  
**statistics**(stats)  
- Code Statistics
- Code Statistics Calculator

### Source Annotation Extractor

rake notes  
rake notes:optimize

### api

rake rdoc (Rails 自身的 API)

### test_unit

rake test

还可再细分为 models、helpers、controllers、mailers、integration 和 jobs 等。

### commands

`rails` 可接以下命令：

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

常用的有:  
  - generate
  - console
  - server
  - dbconsole
  - new

还有:
  - destroy
  - plugin new
  - runner

> Note: 迁移相关在 Active Record 里的 databases.rake 里定义。

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

## 路径

包括 Root & Path，但只有 Root 对外提供接口。

```
add

all_paths
autoload_once
autoload_paths
eager_load
load_paths
```

## Rack

2 个 Rack application.

Debugger 和 Logger

## rails console 里的小技巧

app、new_session 和 reload!

helper 和 controller
