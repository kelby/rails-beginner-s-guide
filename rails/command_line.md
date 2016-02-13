由 rake 和 rails 两部分组成。

## rake

#### tasks.rb 及 tasks 目录

都有单独的 rake 文件，`rake -T` 包含但不限于这下面的命令：

annotations(notes)  
initializers  
framework(rails)  
log  
middleware  
misc(secret、about、time)  
routes  
tmp  
rails(update、template)  
restart  
engine(app、db)  
**statistics**(stats)  
- Code Statistics
- Code Statistics Calculator  

Rakefile 里的 `Rails.application.load_tasks` 会加载本应用使用到第三方 Railtie 和 Engine，及 Rails 自身(按照约定 lib/tasks 也包含在内)所定义的 rake 任务。

#### Source Annotation Extractor

和上面列举的：
<br>
rake notes  
rake notes:optimize 等相关。

可以只查看和某一命名空间有关的 rake 任务，例如和 notes 相关的任务：

```
$ rake -T notes

rake notes
rake notes:custom
```

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
