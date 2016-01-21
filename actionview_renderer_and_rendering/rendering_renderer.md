#### Renderer - 渲染的入口

**渲染的入口。**(它只是调用，真正的渲染工作不是它做的)

`render` 或 `render_body` 方法加上它们的参数，决定了使用 Template Renderer、Partial Renderer 还是 Streaming Template Renderer.

```ruby
# Main render entry point shared by AV and AC.
render(context, options)

# Render but returns a valid Rack body.
render_body(context, options)

# 下面这两个方法，没有对外提供的接口，不要直接使用它们！

# Direct accessor to template rendering.
render_template(context, options)

# Direct access to partial rendering.
render_partial(context, options, &block)
```

> Note: 说明一下，Streaming Template Renderer 用得很少，本说明后文不再重复。
