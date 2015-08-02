## 渲染器介绍

**渲染器需要有上下文和模板对象，才能顺利的工作。**

(但它不直接和模板打交道？谁和模板打交道？)

### Renderer

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

### Abstract Renderer

**具体某个渲染器的抽象类、基类，定义了接口，并提供了几个保护方法给子类使用。**

目前，只有 3 个渲染器，Template Renderer、Partial Renderer 和 Streaming Template Renderer.

接口：

```
render
```

保护方法：

```
extract_details
prepend_formats
```

除上述外，还有：

```
delegate :find_template, :template_exists?, :with_fallbacks, :with_layout_format,
         :formats, :to => :@lookup_context
```

### 1) Template Renderer

普通的模板渲染器，直接继承于 Abstract Renderer.

用到了 template 里的东西。

要考虑 layout.

借助了 @lookup_context.

### 2) Partial Renderer

局部模板渲染器，直接继承于 Abstract Renderer.

可以渲染一个集合，此时借助了 Partial Iteration.

借助了 @lookup_context.

### 3) Streaming Template Renderer

流模板渲染器，直接继承于 Template Renderer，间接继承于 Abstract Renderer.

重写了父类的 render_template 方法。

Streaming Template Renderer 用得很少，不做过多介绍。

### template/ 目录

包含了：error、handlers、html、resolver、text、types 等。<br>
其中 handlers 又包含了：builder、erb、raw 等。

其中，Html 和 Text 这两个类为模板(较简单)，handlers 下面的 Builder、ERB 和 Raw 3 个类也是模板(较复杂)。

从上述可知，Renderer 其实也没做什么，处理工作交给 Template 进行处理。

```
Renderer(最外层的接口)
      |
      V
各个 Renderer(中间处理)
      |
      V
Template(最底层的处理)
```

template/ 目录里的各个 handler，执行的是与 Rails 环境无关的解析、渲染工作。

### 最后

根据要渲染的内容，render 还有不少的可选参数，比如：:partial、:template、:inline、:file 和 :text，使用的时候需要根据情况挑选使用。
