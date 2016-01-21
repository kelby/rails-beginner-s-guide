### 不怎么常用的设置项

```ruby
# 框架初始化后，运行这里的任务
config.after_initialize takes

# 它里面所包含的 namespace 都会被立即加载
config.eager_load_namespaces

# 和 autoload_paths 区别在于：开发环境下，每一次请求之间，它不会被更新了
# 使用它的目录，必须同时使用 autoload_paths
config.autoload_once_paths

# 每一次请求之间，是否需要重新加载模板
# 默认和 cache_classes 设置的一样
config.action_view.cache_template_loading

# 如果你想把周日当做第一天，那也可以
config.beginning_of_week

# 打印日志里是否显示颜色
config.colorize_logging

# 给 log 打标签
# 子域名或多服务器时可考虑
config.log_tags

# 决定了报错时，是否直接在 Web 上显示错误堆栈信息
config.consider_all_requests_local
```
