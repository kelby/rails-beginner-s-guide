## Parameter Filter & Filter Parameters

**过滤参数。**如：

```ruby
env["action_dispatch.parameter_filter"] = [:password]
# => replaces the value to all keys matching /password/i with "[FILTERED]"
```

这就是为什么我们在日志里看不到 password 的值，而是显示 FILTERED 的原因。

**实例方法：**

```ruby
filter

# 具体包含下面 3 项内容：

filtered_parameters
filtered_env
filtered_path
```

在 new 页面，对 http://blog.test.example.com:3000/users 发起 POST 请求。

检测结果：

```ruby
filtered_env
# => 一大串 env 相关数据

request.filtered_parameters
# => {"utf8"=>"✓",
 "authenticity_token"=>"+.../hHj0C5Ao7CxXYwlejyghpNM3elUVw==",
 "user"=>{"name"=>"kelby", "email"=>""},
 "commit"=>"Create User",
 "controller"=>"users",
 "action"=>"create"}
 
request.filtered_path 
# => "/users"
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

除上述方法外，还有：

```
env_filter

filtered_query_string

parameter_filter_for

parameter_filter
```

> Note: 用得比较多的是方法 parameter_filter，通常用来配置过滤 :password ... 过滤的其实是 ParameterFilter 的实例对象，虽然这里没有体现出来。
