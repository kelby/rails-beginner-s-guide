# Action Dispatch Http

**重点是 request 和 response.**

和 Web 服务器已经很接近了，接收的是 request, 响应的是 response.

每一个请求都有 request 和 response.

request 为 ActionDispatch::Request 的实例对象。<br>
response 为 ActionDispatch::Response 的实例对象。

- Request

包含了 Cache::Request、MimeNegotiation(又涉及 Mime)、Parameters、FilterParameters 和 URL.

以及 Headers、Uploaded File 和 URL.

- Response

包含了 FilterRedirect 和 Cache::Response.

以及 Mime.

**Request**

常用到的 request，它还有很多方法在这。

**Headers**

通过它可以访问当前环境下 HTTP 请求头(将 HTTP headers 转换成环境变量)

**Mime Negotiation**

**Parameters**

常用到的 params (也叫 parameters).

**Uploaded File**

上传文件时用到，不过具体不清楚。

**Parameter Filter && Filter Parameters**

过滤参数。如：

```ruby
env["action_dispatch.parameter_filter"] = [:password]
=> replaces the value to all keys matching /password/i with "[FILTERED]"
```

这就是为什么我们在日志里看不到 password 的值，而是显示 FILTERED 的原因。

**URL**

前面 request 里带 url 或 path 的方法有几个和它相关。

**Response**

常用到的 response.

**Rails Meta Store && Rails Entity Store**

都没有任何文档说明的。

**Mimes && Type**

设置响应类型 Mime Type.

**Filter Redirect**

**Cache**

HTTP Cache 相关。如：设置，检测 ETag、Cache-Control

包含两部分：
Request 和 Response.
