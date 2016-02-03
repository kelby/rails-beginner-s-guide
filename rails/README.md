# railties

#### 结构

**1) Railtie**

对 Rails 本身的改造。

**2) Engine**

对 Rails 外围的扩展。

**3) Application**

初始化时：Bootstrap 在前，Finisher 在后。

与我们应用接头。

实例方法，给 Rails.application 使用。

#### 内容

**1) 配置**

指的是 Railtie, Engine, Application 的 Configuration.

**2) 初始化**

"初始化"这里是名词，主要是对它的使用，如 Application 的 Bootstrap 和 Finisher，以及我们项目 AppName 所涉及到的初始化。

**3) 启动！**

没有额外的"启动"程序，把配置、初始化做好了以后，启动就是自然而然的事了。

#### 继承关系

```ruby
Rails.application.class.ancestors
  => [AppName::Application,
  Rails::Application,
  Rails::Engine,
  Rails::Railtie,
  Rails::Initializable,
  ... ...
  Object,
  ... ...
  Kernel,
  BasicObject] 
```
