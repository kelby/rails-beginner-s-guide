### render 参数汇总

#### ActionController::Rendering

```ruby
# 渲染普通文本，不带格式
:plain

# 渲染 html 格式的内容
:html

# 兼容废除的 :text，默认是普通文本。不推荐使用
:body
```

```ruby
# 指定 status
:status
# 指定 content_type
:content_type
# 指定 location
:location
```

```ruby
# 必需与 block 结合使用，里面可以放 Prototype 相关代码，会调用到 Erubis 的
# JavaScriptGenerator 模块；这是比较老的用法，现在推荐使用 js.erb 的方式。
:update
```

```ruby
# 渲染普通文本，不带格式。不推荐使用
:text

# 不渲染任何东西，已废除。不推荐使用
:nothing
```

#### AbstractController::Rendering

```
:variant
```

#### ActionView::Rendering

```
:variant
:formats

:template
:action
:partial
:file

:prefixes
```

teplate 和 action，也可以根据是否有 "/" 做判断。

#### ActionView::Renderer

```
:partial
```

这里的 render 方法，是 AV and AC 的主要入口，如果有 :partial 则渲染的是局部模板；否则，渲染的是普通模板。

#### ActionView::PartialRenderer

AbstractRenderer 的子类之一。

```
:spacer_template
:layout
:locals
:formats
:partial
:object
:as
:collection
```

#### ActionView::TemplateRenderer

AbstractRenderer 的子类之一。

```
(主要是以下 7 项)
:body
:text
:plain
:html
:file
:inline 和 :type
:template

:layout
:locals
```

#### ActionView::Helpers::RenderingHelper

View 里 render 方法所在地，对外提供接口，处于最外层。

```
:partial
:layout
:locals
```

作用：根据参数是不是 Hash 类型，参数有没有带 block，决定了如何渲染。
<br>
如果不传递 hash，则默认渲染 partial 并且第 2 个及之后的参数做为 locals hash.

#### ActionController::Renderers (Metal 增强模块)

```
:json 和 :callback
:js
:xml
```

#### ActionController::Renderer

```
:https

:method
```
