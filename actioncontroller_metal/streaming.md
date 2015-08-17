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

头部消息加上了：

```
Transfer-Encoding: chunked
```

**如何使用？**

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
一般我们都把 js 和 css 放 head 里，这意味着它们会先于页面内容返回。


**注意事项：**

注意：使用 Streaming 特性后后，HTML 里的 head 部分会先返回，不受页面内容的影响。
如果原来的 head 部分依赖于页面，需要做对应修改。

举例：如果 `title` 是根据页面动态生成的，使用 Streaming 后就会出现缺失的情况，需要更改原有代码。可以改成使用 `provide` 和 `yield`；

```ruby
# projects/index.html.erb
<% provide :title, "Projects" %>
```

```ruby
# layouts/application.html.erb
<title><%= yield :title %></title>
```

再举例：使用此特性后，将不能动态生成 `cookie` 和 `session` 内容。

再举例：并不是所有应用服务器(例如默认的 Webrick)都支持此特性。

使用之前，请搞清楚它要解决的问题，以及带来的新问题。还有，不要和【Live】里的 `stream` 混淆了。

> Note: 可通过命令 curl -i localhost:3000 查看使用效果。
