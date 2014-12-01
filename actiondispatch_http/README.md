# ActionDispatch Http

和 Web 服务器已经很接近了，接收的是 request, 响应的是 response.

```ruby
request =>
  @env = { key1: "value1", key2: value2", ... }
  @filtered_env=nil,
  @filtered_parameters=nil,
  @filtered_path=nil,
  @fullpath=nil,
  @ip=nil,
  @method=nil,
  @original_fullpath=nil,
  @port=nil,
  @protocol=nil,
  @remote_ip=nil,
  @request_method=nil,
  @symbolized_path_params=nil,
  @uuid=nil>
```

**URL**

前面 request 里带 url 或 path 的方法有几个和它相关

**Uploaded File**

上传文件时用到，不过具体不清楚。

**Response**

常用到的 response

**Request**

常用到的 request，它还有很多方法在这

**RailsMetaStore && RailsEntityStore**

都没有任何文档说明的。

**Parameters**

常用到的 params (也叫 parameters)

**Mimes && Type**

设置响应类型 Mime Type

**Mime Negotiation**

**Headers**

通过它可以访问当前环境下 HTTP 请求头(将 HTTP headers 转换成环境变量)

**Filter Redirect**

**Parameter Filter && Filter Parameters**

过滤参数。如：

```ruby
env["action_dispatch.parameter_filter"] = [:password]
=> replaces the value to all keys matching /password/i with "[FILTERED]"
```

这就是为什么我们在日志里看不到 password 的值，而是显示 FILTERED 的原因。

**Cache**

HTTP Cache 相关。如：设置，检测 ETag、Cache-Control

包含两部分：
Request 和 Response

