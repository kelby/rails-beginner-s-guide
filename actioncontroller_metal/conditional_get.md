## Conditional Get - HTTP Cache

**页面相关的客户端缓存**

根据 ETag 和 Last-Modified 来决定是否渲染页面，可充分利用客户端(例如浏览器)的缓存，在 Rails 里属于 Controller#action 层面的缓存。

它并不属于应用层面的缓存，因为它用的是状态头和浏览器的特性，这和 Rails.cache 等缓存机制都不一样。

**类方法：**

```
etag
```

`etag` 把元素加入到 etaggers 里。它只是把元素加入到 etaggers，自身是不影响 HTTP 缓存结果的；因为后面的调用，才影响 HTTP 缓存结果。
<br>
这里添加的元素对当前 Controller 下所有的 action 都会起作用。

**实例方法：**

```
fresh_when

# 还有
expires_in
expires_now

stale?
```

`fresh_when` 根据传入的参数，设置 response 里和 HTTP 缓存有关的字段，并完成响应。只对当前 action 起作用，用到了上面提到的 etaggers，影响 ETag 和 last_modify 的值。

`stale?` 不只是单纯的询问，它用到了 fresh_when，所以除了返回 boolean 外，还会影响 response.

`fresh_when` 和 `stale?` 都可以传递 `:template` 参数以便指定模板。(这部分由 Etag With Template Digest 进行处理)

`expires_in` 设置 response 里和 HTTP 缓存有关的字段。
<br>
`expires_now` 设置 response 里和 HTTP 缓存有关的字段。

注意：HTTP 缓存有关的字段有多个，它们设置的是不同字段，或同字段但不同值。

使用举例：

```ruby
# 默认传递 @post 给 posts/show 模板进行求值，我们希望改变这点，传递 @post 给 widges/show 求值
fresh_when @post, template: 'widgets/show'

# 只对 @post 求值，不带入模板内容
fresh_when @post, template: false
```

支持设置 HTTP header 里的 etag 和 last modified 就意味着当所请求的资源没有更改过时，Rails 可以返回一个的响应。这可以节省你的带宽资源和时间。

> Note: 一般的，etag 相关设置只是节省了中间数据传输的网络资源，但在服务器上的计算并没有减少。
