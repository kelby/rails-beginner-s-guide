## Streaming

**改变渲染顺序。**

尽量不要和 Live 里的 stream 搅在一起！

Rails 默认的渲染过程先是模板，然后才是布局。完成模板，及需要的查询，最后才完成布局。

Streaming 可以改变一下顺序，按布局来渲染。布局先显示，对于用户体验可能更好一点，还有就是这会使得 JS 和 CSS 的加载顺序比平时提前。

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

参考

[#266 HTTP Streaming](http://railscasts.com/episodes/266-http-streaming)
<br>
[Streaming with Rails 4](http://www.sitepoint.com/streaming-with-rails-4/)
