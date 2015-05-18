## Streaming

**改变渲染顺序。**

Rails 默认的渲染过程：先是模板，然后是数据库查询，最后才是布局。

```
HTTP request -> dynamic content generation -> static head generation -> HTTP response
```

具体一点：它先执行 `yield` 里的内容，渲染模板，最后才是加载 assets/layouts.

Streaming 可以改变一下顺序，按布局来渲染。布局先显示，对于用户体验可能更好一点，还有就是这会使得 JS 和 CSS 的加载顺序比平时提前。
变成(这里只是类似)：

```
HTTP request -> static head generation -> HTTP response
             -> dynamic content generation -> HTTP response
```

状况消息加上了：

```
Transfer-Encoding: chunked
```

顺序上：使用 Streaming 后，HTML 里的 head 部分会先返回，不受页面内容的影响；

内容上：如果 head 里的内容是依赖于页面生成的，会受影响。
<br>
比如：title 是根据页面动态生成的，使用 Streaming 后就会出现缺失的情况，需要更改原有代码。可以改成使用 provide 和 yield；
<br>
再比如：将不能动态生成 cookie 和 session

使用之前，请搞清楚它要解决的问题，以及带来的新问题。还有，概念不要和【Live】里的 stream 搅在一起！

Streaming 没有对外提供方法，在 `render` 的时候，加上 `:stream` 参数即可：

```ruby
class PostsController
  def index
    @posts = Post.all
    render stream: true # 仅作用于模板，json、xml 等格式的数据不行
  end
end
```

先返回 HTML 的 head 部分，之后返回 body 部分；而不是像之前全部渲染完全才一起返回 head 和 body.
<br>
因为我们一般都把 js 和 css 连接放 head 里，先返回它们有大益。

引出新问题。body 里定义的 @instance 在 head 里不能用，改成 content_for/yield 也不能用。
<br>
使用 provide/yield 解决。

```ruby
# projects/index.html.erb

<% provide :title, "Projects" %>
```

```ruby
# layouts/application.html.erb

<title><%= yield :title %></title>
```

引出新问题，并不是所有应用服务器(例如默认的 Webkit)都支持。

> Note: 可用命令 curl -i localhost:3000 查看大致使用效果。
