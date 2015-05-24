## render 方法详解

### 散落各地的 render 方法

搜索了一下，API 里 `render` 同名方法有 8 个，它们分别代表什么意思？

而 Action View 就有 6 个 render 方法(其中 2 个和测试有关)，分别在：

- Helpers::RenderingHelper - render(options = {}, locals = {}, &block)

要使用这个模块，你需要实现 `view_renderer` 方法，这个方法返回一个 ActionView::Renderer 对象。

- PartialRenderer - render(context, options, block)

用于渲染局部模板。

- Renderer - render(context, options)

Action View 和 Action Controller 渲染的主要入口。

- Template - render(view, locals, buffer=nil, &block)

剩下两个在 AbstractController::Rendering 和 ActionController::Instrumentation 里。

### render 的各个可选参数

```
spacer_template # 配合 collection 和 partical. 在局部模板之间穿插的内容。(为什么不把它们放到局部模板里，而是用这种奇怪的方式？可能是 partical 还被其它人使用，不能直接添加吧)
layout     # 已编
partial    # 渲染以 _ 开头的局部模板
locals     # 已编
formats
object     # 每一个局部模板都有一个和它同名的变量名可用，使用 object 可设置这个变量的值(类似 locals，但这里只有一个变量，且变量名固定)
as         # 配合 collection. 渲染一个集合后，默认同名的变量名可能不同，我们需要指定，使用 as 可设置这个变量(类似 object)
collection # 集合里的每一个元素都要渲染同一个局部模板，所以可以传递一个集合给局部模板。作用和 collection.each 之后再 render 一样的

layout   # 默认都是使用当前布局，你可以更改或不使用布局
locals   # 传递变量给局部模板，以 Hash 的形式编写，可以传递多个变量，名字自取
body     # 只返回 content，而不关心 content type. 区别于 :plain 和 :html (实际渲染的是 text)
text     # 渲染纯文本内容
plain    # 仅仅为了返回很简单的数据。这里没有 HTML 状态和 layout (实际渲染的是 text, 没有 layout)
html     # 直接渲染代码，默认使用 HTML 格式。不好的实践(实际渲染的是 html, 没有 layout)
file     # 明确指定渲染哪个文件，一般这个 key 可省，传 value 即可。默认不使用 layout (实际是渲染 template)
inline   # 直接渲染代码，默认使用 ERB 格式。不好的实践 (不使用 layout)
template # 明确指定渲染哪个模板，一般这个 key 可省，传 value 即可
prefixes

nothing      # 有时候我们不希望渲染任何东西(渲染的是 text)
status       # 对应着 HTTP status code
content_type # html, json, xml 都是可以自动识别 content type 的，其它格式可能你需要指定
location     # 对应着 HTTP Location header

action # 渲染某个 action
type   # 配合 inline，可以渲染其它类型的代码(默认是 ERB)
```

```ruby
ActionController::Renderers::RENDERERS
 => #<Set: {:json, # 返回 json 格式的数据(使用了 to_json)
 :js, # 直接渲染代码，默认是 JavaScript 格式
 :xml # 返回 xml 格式的数据(使用了 to_xml)
 }> 
```

### render 在 Controller 和 View 是如何工作的

**Controller 里默认渲染的是 template**
走的路是 ActionController::Rendering -> AbstractController::Rendering -> ActionView::Rendering -> ActionView::Renderer#render

**View 里默认渲染的是 partial**
走的路是 ActionView::Helpers::RenderingHelper#render -> ActionView::Renderer

通过上面的路径和特别指出的两个 render 方法里面的逻辑，不难看出为什么可以默认渲染 template 或 partial.

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣。

### 一些使用举例

1) 不想渲染任何东西，可以使用：

```ruby
render nothing: true
```

此时，默认 status = 200, 你也可以手动指定状态码。

2) 不想渲染任何东西，还可以使用方法：

`head `

3) 不想渲染，只想查看结果可以使用 `render_to_string`  
传递给它的参数和 render 一样，但它始终返回一个字符串。

4) 一些常用可选参数

明确指定要渲染的模板，用 template

```ruby
render template: "products/show"
```

明确指定要渲染的文件，用 file

```ruby
render file: "/u/apps/warehouse_app/current/app/views/products/show"
```

渲染文件，默认是没有 layout 的，如果需要，你可以手动指定 `layout: true`

5) 不用模板，但效果类似，用 inline

```ruby
render inline: "<% products.each do |p| %><p><%= p.name %></p><% end %>"
```

默认后面的代码用 ERB 解析，如果需要，你可以手动指定 `type: :builder`

inline 违背了 MVC 模式，实践起来并不友好，不推荐使用。

6) 渲染纯文本，用 plain

```ruby
render plain: "OK"
```

7) 不用模板，但效果类似，用 html

```ruby
render html: "<strong>Not Found</strong>".html_safe
```

和 inline 一样，这违背了 MVC 模式，实践起来并不友好，不推荐使用。

8) 渲染返回 json

```ruby
render json: @product
```

这里的数据会自动转换成 json 格式，不需要调用 to_json

9) 渲染返回 xml

```ruby
render xml: @product
```

这里的数据会自动转换成 xml 格式，不需要调用 to_xml

10) 渲染返回 js

```ruby
render js: "alert('Hello Rails');"
```

11) 渲染，但不指定类型，用 body

```ruby
render body: "raw"
```

### 其它

只渲染 json，只得到数据。

只渲染 js 报错，安全隐患。

Controller 里可以指定渲染 partial，但 View 里不可指定渲染 template.

参考

[method render](http://apidock.com/rails/ActionController/Base/render)<br>
[Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html)<br>
[Action View Partials](http://api.rubyonrails.org/classes/ActionView/PartialRenderer.html)<br>
[Rails View Rendering using Naming Convention](http://jonathanhui.com/ruby-rails-3-view)<br>
[Render Options in Rails 3](https://blog.engineyard.com/2010/render-options-in-rails-3/)<br>
[Ruby on Rails - Render](http://www.tutorialspoint.com/ruby-on-rails/rails-render.htm)
