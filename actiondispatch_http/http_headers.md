# Http Headers

使用举例：

```ruby
env     = { "CONTENT_TYPE" => "text/plain" }
headers = ActionDispatch::Http::Headers.new(env)
headers["Content-Type"] # => "text/plain"
```

对应 Rails 里的 `request.headers` 而不是 `headers`

> Note: 它包含了很多内容(少量env, 一般rack, 很多action_dispatch, 少量action_controller)，感兴趣可以看看。
