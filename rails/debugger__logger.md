## Rack - Debugger & Logger

2 个 Rack 插件：Debugger 和 Logger

使用 Debugger 需要安装 gem 'debugger'. 此外，Debugger 在 Rails 5 将被删除，使用 gem 'byebug' 代替。

Logger 用于日志记录，默认已经添加到 middleware 堆栈：

```
use Rails::Rack::Logger
```
