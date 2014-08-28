
除了 Generators 外，其它模块几乎不能用于 Rails 之外。


# 体系结构

## Application

与我们应用接头。

## Engine

对 Rails 外围的扩展。

## Railtie

对 Rails 本身的改造。

# 作用

### 配置

指的是 Railtie, Engine, Application 的 Configuration

### 初始化

"初始化"这里是名词，主要是对它的使用，如 Application 的 bootstrap 和 finisher，以及我们项目 AppName 所涉及到的初始化。

### 启动！

配置、初始化还不够，还有启动！

参考"启动过程"独立章节。

# 其它

## Generators

所有 generator 相关，比较独立。

### commands

rails 命令行

### tasks

rake 命令行

### Rails

Rails 对象的方法。其实它没有这么重要，但因为名字都是它，所以这里给它加大了比重。




