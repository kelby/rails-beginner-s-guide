## render 参数汇总


### ActionController::Rendering

```
RENDER_FORMATS_IN_PRIORITY = [:body √, :text √, :plain √, :html √]
plain √
update (必需与 block 结合，里面可以放  Prototype 相关代码，会调用到 Erubis 的 JavaScriptGenerator 模块；这是比较老的用法，现在推荐使用 js.erb 的方式)
html √
nothing √
body √
status √
:status √, :content_type √, :location √
```


### AbstractController::Rendering

```
variant
```


### ActionView::Rendering

```
variant
formats
:template, :action (是否有 "/" 也可以做判断依据)
partial
:partial, :file√, :template √
prefixes √
```


### ActionView::Renderer

```
partial
```

这里的 render 方法，是 AV and AC 的主要入口，如果有 :partial 则渲染的是局部模板；否则，渲染的是普通模板。

### ActionView::PartialRenderer

```
spacer_template √
layout √
locals √
formats √
partial √
object √
as √
collection √
```


### ActionView::TemplateRenderer

```
layout
locals
type (需要与 inline 结合，才能使用，默认为 erb)

(主要是以下 7 项)
body
text
plain
html
file √
inline √
template
```


### ActionView::Helpers::RenderingHelper

```
partial
layout
locals
（如果不传递 hash，则默认渲染 partial 并且第 2 个及之后的参数做为 locals hash）
```

View 里 render 方法所在地。

### ActionController::Renderers (Metal 增强组件)

```
json
js
xml

callback (必需与 json 一起使用)
```







