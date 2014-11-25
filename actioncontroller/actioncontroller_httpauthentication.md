## Http Authentication

3 个级别的验证，分别是 Basic、Digest 和 Token.

设置头部消息 "WWW-Authenticate"，获取 `headers["WWW-Authenticate"]`

我们最最常用的是：

```ruby
authenticate_or_request_with_http_basic(realm = "Application", &login_procedure)
```
