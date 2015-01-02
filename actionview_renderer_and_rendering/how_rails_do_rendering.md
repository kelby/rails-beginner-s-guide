## Rails 是如何渲染的

别忘记了在 YourController#your_action 里，除了有 `render` 方法外，还有 N 多和渲染相关的方法！有它们就完全可以做渲染任务了。

在某个 action 里打断点，然后：

```ruby
self.respond_to? :render
=> true

self.respond_to? :lookup_context
=> true

self.respond_to? :view_renderer
=> true

self.respond_to? :view_context
=> true

self.respond_to? :view_assigns
=> true
```

> Note: 在 YourMailer#your_action 里打断点，结果一样。

我们调用的是 `render` 方法时，它会调用到 `lookup_context` 和 `view_renderer`

而 `lookup_context` 可以帮我们找到要渲染的模板。

渲染器 + 模板内容 + 上下文

```
view_renderer.new(@lookup_context).render(context, options, block)
```

`view_renderer` 指的是渲染器，也就是：TemplateRenderer、PartialRenderer 或 StreamingTemplateRenderer.

`lookup_context` 用来查找模板的。

`context` 指的是上下文。对应上面的 view_context.

通过 `self.view_assigns` 或 `self.view_context.assigns` 可以查看，我们在 Controller 传递给 View 的实例变量(它们只是上下文内容里的一部分)。 
