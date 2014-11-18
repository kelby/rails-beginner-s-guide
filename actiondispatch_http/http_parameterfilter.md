## Parameter Filter & Filter Parameters

```
filter
```

和

```
env_filter

filtered_env
filtered_parameters
filtered_path
filtered_query_string

parameter_filter_for

parameter_filter
```

使用举例：

```ruby
# 用 "[FILTERED]" 替换 /password/i
env["action_dispatch.parameter_filter"] = [:password]

# 用 "[FILTERED]" 替换 /foo|bar/i
env["action_dispatch.parameter_filter"] = [:foo, "bar"]
```

以上直接使用 env 比较少见，更多的用法是：

```ruby
# filter_parameter_logging.rb
Rails.application.config.filter_parameters += [:password]
```

查看应用程序过滤了什么字段：

```ruby
Rails.application.config.filter_parameters
```

> Note: 用得比较多的是方法 parameter_filter，通常用来配置过滤 :password ... 过滤的其实是 ParameterFilter 的实例对象，虽然这里没有体现出来。
