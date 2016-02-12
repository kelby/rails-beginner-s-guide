### render 方法详解

#### render 在 Controller 和 View 是如何工作的

**Controller 里默认渲染的是 template**
走的路是 ActionController::Rendering -> AbstractController::Rendering -> ActionView::Rendering -> ActionView::Renderer#render

**View 里默认渲染的是 partial**
走的路是 ActionView::Helpers::RenderingHelper#render -> ActionView::Renderer

通过上面的路径和特别指出的两个 render 方法里面的逻辑，不难看出为什么可以默认渲染 template 或 partial.

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣。

#### 一些使用举例

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

和 :inline 一样，这违背了 MVC 模式，实践起来并不友好，不推荐使用。

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

#### render 单个对象，还是集合

渲染一个集合(传递 :collection)，而不是一个个的渲染对象(传递 :object)。

需要注意：

- 一般要指明 :partial 否则某些情况下会报错。
- 集合里的元素在局部模板里和局部模板同名变量表示。
- 可以用 :as 改变这里的同名变量。(渲染单个对象时，也可以用些参数)
- 额外的参数用 :locals 传递。（上面的同名变量你不用 :as 的话，也可以这么传递）

渲染集合，除了少敲几个字符外，性能也会对应变快一点。

#### 其它

只渲染 json，只得到数据。

只渲染 js 报错，安全隐患。

Controller 里可以指定渲染 partial，但 View 里不可指定渲染 template.
