## 结构

### Application

初始化为：Bootstrap 在前，Finisher 在后。

与我们应用接头。

实例方法，给 Rails.application 使用

### Engine

对 Rails 外围的扩展。

### Railtie

对 Rails 本身的改造。

## 内容

### 配置

指的是 Railtie, Engine, Application 的 Configuration

### 初始化

"初始化"这里是名词，主要是对它的使用，如 Application 的 bootstrap 和 finisher，以及我们项目 AppName 所涉及到的初始化。

### 启动！

配置、初始化还不够，还有启动！

参考"启动过程"独立章节。

## 其它

### Generators

所有 generator 相关，比较独立。

除了 Generators 外，其它模块几乎不能用于 Rails 之外。

### Rails` 的类方法

这里的 Rails` 指的是 class Rails. 因为它与框架同名，所以这里会加大它的重要指数，下面就说说它的类方法。

```
application
backtrace_cleaner
configuration
env, env=
groups
public_path
root
```

```
attr_writer :application
attr_accessor :app_class, :cache, :logger
```

```
delegate :initialize!, :initialized?, to: :application
```
