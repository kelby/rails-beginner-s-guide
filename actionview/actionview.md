布局及渲染 [Layouts and Rendering in Rails](http://edgeguides.rubyonrails.org/layouts_and_rendering.html)



表单比重很大，所有可以参考[Form Helpers](http://edgeguides.rubyonrails.org/form_helpers.html)


## 其它

RoutingUrlFor 里的
```ruby
url_for(options = nil)
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



