## Headers

通过它可以访问当前环境下 HTTP 请求头(将 HTTP headers 转换成环境变量)

数据结构和 Hash 类似，所以提供的方法也类似，这里就不列举了。

使用举例：

```ruby
env     = { "CONTENT_TYPE" => "text/plain" }
headers = ActionDispatch::Http::Headers.new(env)
headers["Content-Type"] # => "text/plain"
```

对应 Rails 里的 `request.headers` 而不是 `headers`

前者，是这里 ActionDispatch::Http::Headers 的实例对象；
而后者，是 ActionDispatch::Response.default_headers 为 Hash 实例对象（特指 Http Response 里的 Headers）

通过它，可获取很多 CGI_VARIABLES 相关值。

> Note: 它包含了很多内容(少量 env, 一般 rack, 很多 action_dispatch, 少量 action_controller)，感兴趣可以看看。
