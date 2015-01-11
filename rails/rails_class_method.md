## Rails` 的类方法

这里的 Rails` 指的是 class Rails. 因为它与框架同名，所以这里会加大它的重要指数，下面就说说它的类方法。

```
application       # 代表我们的应用，它是 AppName::Application 的实例对象
configuration     # 它是 Rails::Application::Configuration 的实例对象
env               # 它是 ActiveSupport::StringInquirer 的实例对象
backtrace_cleaner # 它是 Rails::BacktraceCleaner 的实例对象
root              # 获取项目 root 路径
public_path       # 获取项目 public/ 路径
groups
```

除以上外，还有

```
attr_accessor :app_class, :cache, :logger
```

```
delegate :initialize!, :initialized?, to: :application
```
