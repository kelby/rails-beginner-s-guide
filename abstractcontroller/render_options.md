## render 参数汇总与详解

参数主要有 3 类：渲染格式、指定内容、传递变量。

#### ActionController::Rendering

```ruby
# 渲染普通文本，不带格式
:plain
# 举例 render plain: "OK"
```

```ruby
# 渲染 html 格式的内容。不推荐使用，可用 html.erb 代替
:html
# 举例 render html: "<strong>Not Found</strong>".html_safe

# 渲染普通文本，不带格式。不推荐使用，可用 :plain 代替
:text

# 兼容废除的 :text，默认是普通文本。不推荐使用
:body

# 不渲染任何东西，已废除。不推荐使用，可用 :plain 代替
:nothing
```

> Note：不想渲染任何东西，还可以使用方法 `head` 或 `render_to_string`

```ruby
# 指定 Header status
:status
# 指定 Header content_type
:content_type
# 指定 Header location
:location
```

```ruby
# 必需与 block 结合使用，里面可以放 Prototype 等代码。不推荐使用
:update
```

#### AbstractController::Rendering

```ruby
# 指定优先渲染的变种
:variant
```

例如，渲染 show action，但 `request.variant = :phone` 则优先渲染：

```
show.html+phone.erb
```

#### ActionView::Rendering

```ruby
# 指定优先渲染的变种
:variant
# 指定渲染的格式
:formats

# 指定要渲染的模板
:template
# 指定要渲染的 action，它会重新确认对应的模板
:action
# 指定要渲染的局部模板
:partial
# 指定要渲染的文件
:file

# 渲染路径前面部分
:prefixes
# 举例 render :show, prefixes: 'posts'
# 类似 render "posts/show"
```

#### ActionView::Renderer

```ruby
# 指定要渲染的局部模板
:partial
```

这里的 render 方法，是 AV and AC 的主要入口，如果有 `:partial` 则渲染的是局部模板；否则，渲染的是普通模板。

#### ActionView::PartialRenderer

AbstractRenderer 的子类之一。

```ruby
# 配合 :collection，渲染局部模板时，各个局部模板之间穿插的内容
:spacer_template

# 布局
:layout

# 传递变量
:locals

# 指定渲染格式
:formats

# 指定要渲染的局部模板
:partial
```

渲染"局部模板"时，约定：每一个局部模板都有一个与之同名的"局部变量"。

例如：

```ruby
render :partial => "person"
```

则在局部模板内有 `person` 可用，它代表什么内容 ，以及如何更改名字。和以下几个参数有关：

```ruby
# 传递单个对象时，与局部模板同名的变量
:object
# 传递的对象别名，更改默认的局部变量名字
:as
# 传递的集合，每一项可用局部变量代替
:collection
```

> Note：前面提到的参数 `:locals` 也可传递变量，它们本质是一样的。

#### ActionView::TemplateRenderer

AbstractRenderer 的子类之一。

```ruby
# 渲染普通文本
:plain

# 渲染 html 格式的内容
:html

# 渲染文件，一般是可下载的文件(不使用 layout)
# 你可以手动指定，例如 layout: true
:file

# 直接渲染代码(不使用 layout)，默认使用 ERB 格式。不推荐使用
:inline
# 举例 render inline: "<% products.each do |p| %><p><%= p.name %></p><% end %>"
# 必须配合 :inline，指定要渲染代码的类型
:type
# 举例 type: :builder

# 指定模板
:template

# 指定布局
:layout

# 传递变量
:locals
```

```ruby
# 兼容 :text，默认是普通文本。不推荐使用
:body
# 渲染普通文本。不推荐使用
:text
```

#### ActionView::Helpers::RenderingHelper

View 里 render 方法所在地，对外提供接口，处于最外层。

```ruby
# 指定局部模板
:partial
# 指定布局
:layout

# 传递变量
:locals
```

作用：根据参数是不是 Hash 类型，参数有没有带 block，决定了如何渲染。
<br>
如果不传递 hash，则默认渲染 partial 并且第 2 个及之后的参数做为 locals hash.

#### ActionController::Renderers (Metal 增强模块)

```ruby
# 渲染内容为 JSON 格式。可用 json.jbuilder 代替
:json
# 举例 render json: @product
# 渲染内容为 JSON 格式时，额外提供的参数
:callback

# 渲染内容为 JS 格式。可用 js.erb 代替
:js
# 举例 render js: "alert('Hello Rails');"

# 渲染内容为 XML 格式
:xml
举例 render xml: @product
```

#### ActionController::Renderer

直接渲染模板，不依赖于具体 Controller#action 时，可额外传递的参数：

```ruby
# 指定请求是否 HTTPS
:https

# 指定请求方法
:method
```
