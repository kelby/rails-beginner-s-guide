# rendering 渲染器介绍

## Renderer

渲染的入口。

**render 或 render_body** 方法加上它们的**参数**，决定了使用 TemplateRenderer、PartialRenderer 还是 StreamingTemplateRenderer.

```ruby
# Main render entry point shared by AV and AC.
def render(context, options)
  if options.key?(:partial)
    render_partial(context, options)
  else
    render_template(context, options)
  end
end

# Render but returns a valid Rack body.
def render_body(context, options)
  if options.key?(:partial)
    [render_partial(context, options)]
  else
    StreamingTemplateRenderer.new(@lookup_context).render(context, options)
  end
end

# 下面这两个方法，没有对外提供的接口，不要直接使用它们！

# Direct accessor to template rendering.
def render_template(context, options) #:nodoc:
  TemplateRenderer.new(@lookup_context).render(context, options)
end

# Direct access to partial rendering.
def render_partial(context, options, &block) #:nodoc:
  PartialRenderer.new(@lookup_context).render(context, options, block)
end
```

> Note: 说明一下，StreamingTemplateRenderer 用得很少，本说明后文不再重复。

## AbstractRenderer

具体某个渲染器的基类，定义了接口。

目前，只有 3 个渲染器，TemplateRenderer、PartialRenderer 和 StreamingTemplateRenderer.

## 1. TemplateRenderer

普通的模板渲染器，直接继承于 AbstractRenderer.

用到了 template 里的东西。

要考虑 layout.

借助了 @lookup_context.

## 2. PartialRenderer

局部模板渲染器，直接继承于 AbstractRenderer.

可以渲染一个集合，此时借助了 PartialIteration.

借助了 @lookup_context.

## 3. StreamingTemplateRenderer

流模板渲染器，直接继承于 TemplateRenderer，间接继承于 AbstractRenderer.

重写了父类的 render_template 方法。

StreamingTemplateRenderer 用得很少，不做过多介绍。

## 最后

You invoked render but did not give any of :partial, :template, :inline, :file or :text option.
