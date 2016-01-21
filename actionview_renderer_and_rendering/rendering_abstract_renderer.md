#### 0) Abstract Renderer

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
