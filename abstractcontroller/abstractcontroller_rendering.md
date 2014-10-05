## Rendering

`render(*args, &block)` Controller 里的渲染。

封装了 ActionView 里的 TemplateRenderer 和 PartialRenderer，提供 render 给 ActionController, ActionMailer 渲染模板文件或局部模板。

真正的渲染工作并不是它完成，它只是封装了 View 里的渲染方法。所以，使用过程中有疑问的，可以查看 View 里对应 render 方法的文档。

1) 不想渲染任何东西，可以使用：

```ruby
render nothing: true
```

此时，默认 status = 200, 你也可以手动指定状态码。

2) 不想渲染任何东西，还可以使用方法：

`head 

3) 不想渲染，只想查看结果可以使用 `render_to_string`  
传递给它的参数和 render 一样，但它始终返回一个字符串。

4) 一些常用可选参数

明确指定要渲染的模板，用 template

```
render template: "products/show"
```

明确指定要渲染的文件，用 file

```
render file: "/u/apps/warehouse_app/current/app/views/products/show"
```

沉浸文件，默认是没有 layout 的，如果需要，你可以手动指定 `layout: true`

5) 不用模板，但效果类似，用 inline

```
render inline: "<% products.each do |p| %><p><%= p.name %></p><% end %>"
```

默认后面的代码用 ERB 解析，如果需要，你可以手动指定 `type: :builder`

inline 违背了 MVC 模式，实践起来并不友好，不推荐使用。

6) 渲染纯文本，用 plain

```
render plain: "OK"
```

7) 不用模板，但效果类似，用 html

```
render html: "<strong>Not Found</strong>".html_safe
```

和 inline 一样，这违背了 MVC 模式，实践起来并不友好，不推荐使用。

8) 渲染返回 json

```
render json: @product
```

这里的数据会自动转换成 json 格式，不需要调用 to_json

9) 渲染返回 xml

```
render xml: @product
```

这里的数据会自动转换成 xml 格式，不需要调用 to_xml

10) 渲染返回 js

```
render js: "alert('Hello Rails');"
```

11) 渲染，但不指定类型，用 body

```
render body: "raw"
```
