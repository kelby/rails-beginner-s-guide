## Railtie

Railtie 是 Rails 的核心部分之一。通过它，可以扩展和修改 Rails 的初始化程序。

每一个 Rails 组件(如：ActionMailer, ActionController,
ActionView 和 ActiveRecord等)都属于 Railtie. 因为它们都需要自己的初始化程序。

什么时候需要使用 Railtie? 当你的扩展符合下列情况时，可以考虑：

* 替换默认组件
* Rails 启动时即要配置内容
* Rails 启动时即要初始化内容

### Railtie 只是配置及初始化文件

Railtie 不属于真正意义上的”代码”，代码已经完成。想把它运用到 Rails 项目里，并且扩展或修改 Rails 的配置、初始化过程，才需要引进 Railtie.

一个 gem 是 Raitie，通常是指它有 raitie.rb (再准确点，有类继承于 Rails::Railtie) … 并不影响它的其它代码。

通常你的项目代码是单独存放的，raitie.rb 只是针对 Rails 项目初始化或配置工作，不推荐把项目代码放到这里。

### Engine 和 Application

Engine 是 Railtie 的子类，所以这里的方法，由于继承关系，它也可以使用。  
Application 是 Engine 的子类，所以这里的方法，由于继承关系，它也可以使用。

### 其它

查看本项目下，所有的 Railtie：

```ruby
Rails.application.send(:ordered_railties)
```

Rails 启动是一个复杂的过程，你不必知道具体在哪一步执行 Railtie 代码。
