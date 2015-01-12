## Rack - Debugger & Logger

2 个 Rack application.

Debugger 和 Logger

使用 Debugger 需要安装 gem 'debugger'<br>
Debugger 在 Rails 5 将被删除，使用 gem 'byebug' 代替。

Logger 用于日志记录，已经添加到默认 middleware：

```
use Rails::Rack::Logger
```
