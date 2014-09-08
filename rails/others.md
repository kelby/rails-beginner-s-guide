# Others

Railtie 不属于真正意义上的”代码”，代码已经完成。想把它运用到 Rails 项目里，并且扩展或修改 Rails 的初始化过程，才需要引进 Railtie.

Rails 启动是一个复杂的过程，你不必知道具体在哪一步执行 Railtie 代码。

一个 gem 是 Raitie，通常是指它有 raitie.rb (再准确点，有类继承于 Rails::Railtie) … 并不影响它的其它代码。

通常你的项目代码是单独存放的，raitie.rb 只是针对 Rails 项目初始化或配置工作，不推荐把项目代码放到这里。

## 其它

查看本项目下，所有的 Railtie：

```ruby
Rails.application.send(:ordered_railties)
```
