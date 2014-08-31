# ConditionalGet

Rails 3.1 Moved etag responsibility from ActionDispatch::Response to the middleware stack.

```ruby
# 单独使用没意义
etag(&etagger) - 追加元素到 etag

# 决定是否改变。Rails 4 以后会对页面内容进行 MD5 求值，fresh_when 设置不合理的话，会得到旧数据。
fresh_when(record_or_options, additional_options = {}) - 满足etag刷新条件，才执行后面操作

# 还有
expires_in(seconds, options = {})
expires_now()
stale?(record_or_options, additional_options = {})
```

`fresh_when` 是实例方法，只当前 action 起作用。比较关键，影响 ETag 和 last_modify 的值。
`etag` 是类方法，对当前 Controller 下面的所有 action 都起作用。就相当于一个过滤器，把里面的元素加入到 record_or_options 里。同样影响 ETag 和 last_modify 的值。

```
X-Runtime	219
Date	Fri, 02 May 2014 13:39:45 GMT
Content-Encoding	gzip
Vary	Accept-Encoding
Server	ITeye SERVER

ETag	"a6ae1d7498b3a3963e7af3ff57e7f3ec"

Transfer-Encoding	chunked
Content-Type	text/html; charset=utf-8
Cache-Control	private, max-age=0, must-revalidate
```

支持设置 HTTP header 里的 etag 和 last modified 就意味着当所请求的资源没有更改过时，Rails 可以返回一个的响应。这可以节省你的带宽资源和时间。

和所有缓存一样，怎么生成 cache_key ？

最新，表示没有更改。

> Note: 注意 ETag 和已经被废除 cache_page 的区别，前者是Web服务器级别，后者是应用服务器级别。如果页面没有更改，ETag 返回的是 "304 Not Modified"，其他什么都不用干，连网络带宽都省了。而 cache_page 还要读取 public/ 目录下的静态HTML文件。

什么也没改 f9d7568ac634fc9ad270f2348d5f3b41
更改表态内容 d9cc40e9e225a3e738c68b01f17ef4b0
update_columns 7128cbb34d84aa096ee62d69d1599a32
update_attributes 98eac754b15c61f4c8b4f79b7f0a5645

> Note: 默认 动态或静态内容有变，ETag 都会更新；都不改变，才不更新。如果手动设置了 fresh_when 反而会获取到旧数据。

ETag 方面的知识，基于资源的HTTP Cache的实现介绍。感兴趣可以查看 [在你的 Rails App 中开启 ETag 加速页面载入同时节省资源](http://huacnlee.com/blog/use-etag-in-your-rails-app-to-speed-up-loading/)
