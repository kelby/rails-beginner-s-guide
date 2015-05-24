## URL

**前面 request 里带 url 或 path 的方法有几个和它相关。**

在 new 页面，对 http://blog.test.example.com:3000/users 以起 POST 请求。

**检测结果(它们是实例方法)：**

```ruby
request.domain
# => "example.com"

request.host
# => "blog.test.example.com"

request.host_with_port # 取得带端口的主机名
# => "blog.test.example.com:3000"

request.optional_port
# => 3000

request.port
# => 3000

request.port_string
# => ":3000"

request.protocol # 取得当前使用网络协议
# => "http://"

request.raw_host_with_port # 代理服务器的主机名和端口
# => "blog.test.example.com:3000"

request.server_port
# => 3000

request.standard_port # 返回网络协议标准端口(http 为 80, https 为 443)
# => 80

request.standard_port? # 判断当前协议是否是标准端口
# => false

request.subdomain
=> "blog.test"
request.subdomain 2
# => "blog"

request.subdomains
# => ["blog", "test"]
request.subdomains 2
# => ["blog"]

request.url # 取得当前 requset 完整 url
# => "http://blog.test.example.com:3000/users"
```

**除上述方法外，还有(模块方法)：**

```
extract_domain
extract_subdomain
extract_subdomains

full_url_for
path_for
url_for
```

以 ActionDispatch::Http::URL.x 的形式调用它们。

**数据来源**

```ruby
env["HTTP_X_FORWARDED_HOST"]
env['HTTP_HOST'], env['SERVER_NAME'], env['SERVER_ADDR'], env['SERVER_PORT']

以及父类 Rack::Request (它也是从 env 里取数据，然后处理。具体不在此描述)
```
