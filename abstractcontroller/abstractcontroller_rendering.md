## Rendering

Controller 和 Mailer "渲染"的入口，并负责转递实例变量。

渲染任务，Abstact Controller 做的只是最外层的封装。具体由 Action View 的 Rendering 相关模块，然后给 Action Controller 和 Active Mailer 使用。

```
Action Controller & Active Mailer
                 |
                 V
          Abstact Controller
                 |
                 V
            Action View
```

它封装了 Action View 里的 Template Renderer 和 Partial Renderer，提供 render 给 Action Controller, Action Mailer 渲染模板文件或局部模板。

我们在 Controller#actions 里使用的 `render` 就是在这里定义的（尽管它只是简单调用，几乎没做任何事）。

所以，在使用 `render` 过程中有疑问的话，可以查看 View 里对应 render 方法的文档。

除上述方法外，还有：

```
view_assigns
```

`view_assigns` 通过它可以查看 Controller 和 Mailer 传递给 View 的实例变量及其内容。在 ActionView::Rendering 里被调用。

```
render_to_body
render_to_string
```

```
rendered_format
```

这些方法主要起到接口的作用，具体实现在上游或下游。
