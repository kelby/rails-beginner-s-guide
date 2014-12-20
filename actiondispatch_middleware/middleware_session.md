**Session**

包括 CacheStore && CookieStore && MemCacheStore

用得最多，并且默认的是 CookieStore.

可以在 config/initializers/session_store.rb 配置你的 session 存储方式:

```ruby
Rails.application.config.session_store :cookie_store, key: '_your_app_session'
```

可以在 config/secrets.yml 配置生成 session 的密钥:

```ruby
development:
  secret_key_base: 'secret key'
```

(密钥除签名、加密 session 外，还有其它用途)

运行 `rake secret`. 会生成随机字符串，你可以使用它们做为 secret_key.

> Note: 如果你更改了 secret_key 的话，之前的 session 就不能使用了。因为 secret_key 还有其它用途，一般不建议更改。
