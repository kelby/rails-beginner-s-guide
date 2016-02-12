### render 参数汇总

#### ActionController::Rendering

```
:plain
:update
:html
:body
:text

:status, :content_type, :location
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
