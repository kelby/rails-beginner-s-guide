由 rake 和 rails 两部分组成。

## rake

#### tasks.rb 及 tasks 目录

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

#### Source Annotation Extractor

rake notes  
rake notes:optimize

#### api

rake rdoc (Rails 自身的 API)

#### test_unit

rake test

还可再细分为 models、helpers、controllers、mailers、integration 和 jobs 等。

## rails

#### commands

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
