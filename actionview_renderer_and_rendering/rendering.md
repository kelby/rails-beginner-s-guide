## 渲染器介绍

**渲染器需要有上下文和模板，才能顺利的工作。**

(但它不直接和模板打交道？谁和模板打交道？)

### Renderer

渲染的入口。

render 或 render_body 方法加上它们的参数，决定了使用 TemplateRenderer、PartialRenderer 还是 StreamingTemplateRenderer.

```ruby
# Main render entry point shared by AV and AC.
def render(context, options)

# Render but returns a valid Rack body.
def render_body(context, options)

# 下面这两个方法，没有对外提供的接口，不要直接使用它们！

# Direct accessor to template rendering.
def render_template(context, options)

# Direct access to partial rendering.
def render_partial(context, options, &block)
```

> Note: 说明一下，StreamingTemplateRenderer 用得很少，本说明后文不再重复。

### Abstract Renderer

具体某个渲染器的基类，定义了接口。

目前，只有 3 个渲染器，Template Renderer、Partial Renderer 和 Streaming Template Renderer.

### Template Renderer

普通的模板渲染器，直接继承于 AbstractRenderer.

用到了 template 里的东西。

要考虑 layout.

借助了 @lookup_context.

### Partial Renderer

局部模板渲染器，直接继承于 AbstractRenderer.

可以渲染一个集合，此时借助了 PartialIteration.

借助了 @lookup_context.

### Streaming Template Renderer

流模板渲染器，直接继承于 TemplateRenderer，间接继承于 AbstractRenderer.

重写了父类的 render_template 方法。

StreamingTemplateRenderer 用得很少，不做过多介绍。

### 最后

根据要渲染的内容，render 还有不少的可选参数，比如：:partial、:template、:inline、:file 和 :text，使用的时候需要根据情况挑选使用。
