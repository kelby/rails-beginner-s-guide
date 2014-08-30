两大块：布局及渲染 [Layouts and Rendering in Rails](http://edgeguides.rubyonrails.org/layouts_and_rendering.html)，辅助方法(尤为重要)

##辅助方法
---------

The \Rails framework provides a large number of helpers for working with assets, dates, forms, numbers and model objects, to name a few. These helpers are available to all templates by default.

```
 RecordTagHelper
 AssetTagHelper
 AtomFeedHelper
 BenchmarkHelper
 CaptureHelper
 FormOptionsHelper
 FormTagHelper
 JavaScriptHelper
 NumberHelper
 DebugHelper
 CacheHelper
 DateHelper
 FormHelper
```

暂，按 HTML 是否自带区分。

> **NOTE:** 表单比重很大，所有可以参考[Form Helpers](http://edgeguides.rubyonrails.org/form_helpers.html)

## 布局及渲染

### layout

`layout(layout, conditions = {})` Specify the layout to use for this class.
我们平时在 Controller 里使用的 layout 方法，其实就是在这里定义的。

### render

```
Returns the result of a render that's dictated by the options hash. The primary options are:

:partial - See ActionView::PartialRenderer.

:file - Renders an explicit template file (this used to be the old default), add :locals to pass in those.

:inline - Renders an inline template similar to how it's done in the controller.

:text - Renders the text passed in out.

:plain - Renders the text passed in out. Setting the content

type as text/plain.

:html - Renders the html safe string passed in out, otherwise

performs html escape on the string first. Setting the content type as text/html.

:body - Renders the text passed in, and inherits the content

type of text/html from ActionDispatch::Response object.

If no options hash is passed or :update specified, the default is to render a partial and use the second parameter as the locals hash.
```

## 其它

ActionView底层我们就不多说了，我们使用过程中接触到的东西并不多，除了上述 helper 外，说一些其它的方法凑数字吧。

ViewPaths 里的

```ruby
append_view_path(path) - 追加路径到当前 Controller 所在的 view paths 里。当我们的自己创建或者使用的 gem 引入的文件目录结构和Rails默认不一样，但且希望渲染到视图里的模板(局部模板)，就会报错，此方法可以帮助我们。
prepend_view_path(path)
```

RoutingUrlFor 里的
```ruby
url_for(options = nil)
```

RecordIdentifier 里的

```ruby
dom_class(record_or_class, prefix = nil)
dom_id(record, prefix = nil)
```

Layouts 里的

```ruby
layout(layout, conditions = {})
```

ActionView 有 4 个 render 方法(不包含测试里的)，分别在：

- Helpers::RenderingHelper - render(options = {}, locals = {}, &block)

In order to use this module, all you need is to implement view_renderer that returns an ActionView::Renderer object.
- PartialRenderer - render(context, options, block)

Used for rendering partials
- Renderer - render(context, options)

Main render entry point shared by AV and AC.
- Template - render(view, locals, buffer=nil, &block)


## Digestor

对视图是如何做加密的，主要用于 cache 自动过期。

