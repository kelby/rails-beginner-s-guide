## Conditional Get 其它

#### 相关概念

1) ETag 过程如下：

- 客户端请求一个页面 A，服务器返回页面 A，并在给 A 加上一个 ETag.<br>
- 客户端展现该页面，并将页面连同 ETag 一起缓存。<br>
- 客户再次请求页面 A，并将上次请求时服务器返回的 ETag 一起传递给服务器。<br>
- 服务器检查该 ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应 304（未修改——Not Modified）和一个空的 response body.

2) Last-Modified 过程如下：

在浏览器第一次请求某一个 URL 时，服务器端的返回状态会是 200，内容是客户端请求的资源，同时有一个 Last-Modified 的属性标记此文件在服务期端最后被修改的时间，格式类似这样：  

```
Last-Modified : Fri , 12 May 2006 18:53:33 GMT  
```

客户端第二次请求此 URL 时，根据 HTTP 协议的规定，浏览器会向服务器传送 If-Modified-Since 报头，询问该时间之后文件是否有被修改过：  

```
If-Modified-Since : Fri , 12 May 2006 18:53:33 GMT  
```

如果服务器端的资源没有变化，则自动返回 HTTP 304 (Not Changed) 状态码，内容为空，这样就节省了传输数据量。当服务器端代码发生改变或者重启服务器时，则重新发出资源，返回和第一次请求时类似。从而保证不向客户端重复发出资源，也保证当服务器有变化时，客户端能够得到最新的资源。

3) 一个典型的 Response headers:

```
Last-Modified: Sun, 09 Nov 2014 05:25:32 GMT 
Etag: "4808a7a249c9fd981be8ba390f55ce8a"

Cache-Control: max-age=0, private, must-revalidate 
Set-Cookie: _sample_app_session=some-thing%3D%3D--some-thing-else; path=/; HttpOnly 
X-Request-Id: 02d61994-90aa-41e4-be96-b65b68914c13
X-Runtime: 0.005787
Server: thin 1.6.2 codename Doc Brown 
Date: Sun, 09 Nov 2014 05:42:53 GMT 
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Content-Type: text/html; charset=utf-8 
Content-Length: 665 
```

#### 和所有缓存一样，怎么生成 cache_key ？

最新，什么也没改。

```
f9d7568ac634fc9ad270f2348d5f3b41
```

更改表态内容：

```
d9cc40e9e225a3e738c68b01f17ef4b0
update_columns 7128cbb34d84aa096ee62d69d1599a32
update_attributes 98eac754b15c61f4c8b4f79b7f0a5645
```

> Note: 默认动态或静态内容有变，ETag 都会更新；都不改变，才不更新。如果手动设置了 fresh_when 反而会获取到旧数据。

如果 fresh_when 匹配，将直接返回结果给最终用户。但后面的代码并未退出，还会执行，只到遇到结束标识为止。也就是说 Controller#action 后续有代码的话，则还会执行，但 View 里的代码不会执行了

这里区别于响应：

```sh
# 之前，还需要渲染视图
Completed 200 OK in 84ms (Views: 10.7ms | ActiveRecord: 2.7ms)

# 之后，不需要渲染视图
Completed 304 Not Modified in 3ms (ActiveRecord: 0.1ms)
```

#### 相关代码

`fresh_when` 方法：

```ruby
def fresh_when(record_or_options, additional_options = {})
  # ... 参数处理，略

  # 设置 response 部分属性
  response.etag = combine_etags(options) if options[:etag] || options[:template]
  response.last_modified = options[:last_modified] if options[:last_modified]
  response.cache_control[:public] = true           if options[:public]

  # 调用 head 方法，返回状态码 304
  head :not_modified if request.fresh?(response)
end
```

可选参数 etag、template、last_modified 和 public.

`head` 方法：

```ruby
def head(status, options = {})
  # ... 设置 status、location、content_type 等，略

  if include_content?(self._status_code)
    self.content_type = content_type || (Mime[formats.first] if formats)
    self.response.charset = false if self.response
    self.response_body = " "
  else
    headers.delete('Content-Type')
    headers.delete('Content-Length')
    self.response_body = ""
  end
end
```

`request.fresh?` 方法：

```ruby
def fresh?(response)
  last_modified = if_modified_since
  etag          = if_none_match

  return false unless last_modified || etag

  success = true
  success &&= not_modified?(response.last_modified) if last_modified
  success &&= etag_matches?(response.etag) if etag
  success
end
```

#### 其它

fresh_when 可以通过工具(如：Chrome 插件"Advanced REST client")查看 ETag，检测是否起作用以及是否正确。

> Note: ETag 和已经被废除 cache_page 的区别，前者是 Web 服务器级别，后者是应用服务器级别。如果页面没有更改，ETag 返回的是 "304 Not Modified"，其他什么都不用干，连网络带宽都省了。cache_page 还要读取 public/ 目录下的静态 HTML 文件，减少了计算过程，但还是需要网络传输页面内容。
