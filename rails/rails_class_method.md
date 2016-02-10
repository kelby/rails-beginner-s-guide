## Rails 文件下的内容

这里指的是 class Rails. 因为它与框架同名，所以这里会加大它的重要指数，下面就说说我们常用的几个类方法。

| 类方法 | 解释 |
| -- | -- |
| application | 代表我们的应用，它是 AppName::Application 的实例对象 |
| configuration | 我们应用的配置相关信息，它是 Rails::Application::Configuration 的实例对象 |
| env | 我们应用所处环境，它是 ActiveSupport::StringInquirer 的实例对象 |
| backtrace_cleaner | 它是 Rails::BacktraceCleaner 的实例对象 |
| root | 获取项目 root 路径 |
| public_path | 获取项目 public/ 路径 |
| cache | 缓存实例对象 |
| logger | 日志实例对象 |
