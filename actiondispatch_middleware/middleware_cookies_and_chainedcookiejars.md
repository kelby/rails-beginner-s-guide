### Cookies & ChainedCookieJars

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
