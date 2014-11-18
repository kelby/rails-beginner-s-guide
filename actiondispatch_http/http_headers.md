## Headers

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

> Note: 它包含了很多内容(少量env, 一般rack, 很多action_dispatch, 少量action_controller)，感兴趣可以看看。
