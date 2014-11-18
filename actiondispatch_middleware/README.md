# ActionDispatch Middleware

已经进入 Rails, 但还没正式进入我们应用，这时候需要做一些事。

Web 服务器已经处理过，转发过来了。做一些什么事呢？看一看下面的 middleware 都提供了什么功能就知道了。

下面只是做简介，详情可以查看 API 说明，或阅读源代码。

**Static**

从硬盘里直接返回静态文件内容。

如果移除此 middleware，则处理方式和普通请求一样，由路由决定转发到哪。

默认这些静态文件存放在 `public/` 目录下，如果找不到文件，则处理方式和普通请求一样，由路由决定转发到哪。

其它特点：自动校验文件路径，不能用于目录遍历等。

**~~Middleware Stack~~**

**~~SSL~~**

**Show Exceptions**

程序抛异常时，如何处理。默认显示错误页面。相关 HTTP response 是 X-Cascade 标识。

**Request Id**

为每个用户的请求生成唯一的 X-Request-Id,  可以在防火墙、负载均衡、Web服务器之间传递。可用于跟踪用户，方便日志处理；或查看请求在集群中哪台机器上处理。

Rails 里可通过 `request.uuid` 查看，相关 HTTP header 是 X-Request-Id 标识。

根据经验，实际项目中可以考虑将此 middleware 移除。

**Remote Ip**

可用于防止IP伪造。

相关 HTTP header 是 X-Forwarded-For (XFF) 标识。

鉴于伪造这一字段非常容易，实际项目中可以考虑将此 middleware 移除。

**Reloader**

ActionDispatch::Reloader 

用于是否缓存类和模块，如果不缓存，则每次请求都要加载类和模块。默认开发环境为 false，这样我们更改代码后，不需要重启就能看到效果。而测试、生产环境为 true.

对应 `config.cache_classes = false`

**Public Exceptions**

程序抛异常时，负责'渲染'错误页面这个工作。默认从 `public/` 目录中寻找对应的异常文件，如 500 则渲染 `/public/500.html`

**~~Params Parser~~**

解析请求过来的参数。

**Flash**

这里是 Flash 消息的实现，至于如何使用，可以查看"Flash"章节

**~~Exception Wrapper~~**

抛异常，该如何报错：

```ruby
ActionDispatch::ExceptionWrapper.rescue_responses

=> {"ActionController::RoutingError"=>:not_found,
  "AbstractController::ActionNotFound"=>:not_found,
  "ActionController::MethodNotAllowed"=>:method_not_allowed,
  "ActionController::NotImplemented"=>:not_implemented,
  "ActionController::InvalidAuthenticityToken"=>:unprocessable_entity,
  "ActiveRecord::RecordNotFound"=>:not_found,
  "ActiveRecord::StaleObjectError"=>:conflict,
  "ActiveRecord::RecordInvalid"=>:unprocessable_entity,
  "ActiveRecord::RecordNotSaved"=>:unprocessable_entity}
  
ActionDispatch::ExceptionWrapper.rescue_templates
=> {"ActionView::MissingTemplate"=>"missing_template",
  "ActionController::RoutingError"=>"routing_error",
  "AbstractController::ActionNotFound"=>"unknown_action",
  "ActionView::Template::Error"=>"template_error"}
```

**Debug Exceptions**

负责在开发环境下，在页面上输出日志和 debug 信息。

**Cookies & ChainedCookieJars**

可以通过 ActionController#cookies 读/写 cookies 数据。

基本的写(由 Cookies 提供):

```ruby
# 简单的 cookie 数据
# 浏览器关闭则删除
cookies[:user_name] = "david"

# cookie 存储字符串数据，其它格式要转化。
cookies[:lat_lon] = JSON.generate([47.68, -122.37])

# 设置 cookie 数据的生命周期为一小时
cookies[:login] = { value: "XJ-122", expires: 1.hour.from_now }
```

高级的写(由 ChainedCookieJars 提供)：

```ruby
# cookie 数据签名(用到secrets.secret_key_base)，防止用户篡改。
# 可以使用 cookies.signed[:name] 获取这签名后的数据
cookies.signed[:user_id] = current_user.id

# 设置一个"永久" cookie，默认生命周期是 20 年。
cookies.permanent[:login] = "XJ-122"

# 你可以链式调用以上方法
cookies.permanent.signed[:login] = "XJ-122"
```

读:

```ruby
cookies[:user_name]           # => "david"
cookies.size                  # => 2
JSON.parse(cookies[:lat_lon]) # => [47.68, -122.37]
cookies.signed[:login]        # => "XJ-122"
```
  
删除:

```ruby
cookies.delete :user_name
```

除上面提到的 signed 和 permanent 外，ChainedCookieJars 还提供方法：

```ruby
# cookie 数据加密(用到 secrets.secret_key_base)，类似于 signed 方法
# 可以使用 cookies.encrypted[:discount] 获取这签名后的数据(已经自动解密)
cookies.encrypted[:discount] = 45

signed_or_encrypted
```

**Callbacks**

`before_call、after_call` 可以在请求转发(request dispatch)前后执行一些回调方法。

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

运行 `rake secret`. 会生成随机字符串，你可以使用它们做为 secret_key

> Note: 如果你更改了 secret_key 的话，之前的 session 就不能使用了。因为 secret_key 还有其它用途，一般不建议更改。
