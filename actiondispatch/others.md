# 这里是源码导读了哦

分为四部分吧。

## Http

- Cache

HTTP Cache 相关。如：设置，检测 ETag、Cache-Control

- FilterParameters & ParameterFilter

过滤参数。如：

```ruby
env["action_dispatch.parameter_filter"] = [:password]
=> replaces the value to all keys matching /password/i with "[FILTERED]"
```
这就是为什么我们在日志里看不到 password 的值，而是显示 FILTERED 的原因。

- Headers

通过它可以访问当前环境下 HTTP 请求头(将 HTTP headers 转换成环境变量)

- mime_type

设置响应类型 Mime Type

- Parameters

常用到的 params (也叫 parameters)

- Request

常用到的 request，它还有很多方法在这

- Response

常用到的 response

- UploadedFile

上传文件时用到，不过具体不清楚。

- URL

前面 request 里带 url 或 path 的方法有几个和它相关

## Middleware

## RouteSet

- 特指 route_set.rb
- 本身就充满魔法
- 还是内外沟通的桥梁
- 内指 Journey
- 外指对外的接口及 routing 目录里的其它内容

## Routing

- 除 route_set.rb 外，routing 目录里的其它内容
- 对外提供接口
