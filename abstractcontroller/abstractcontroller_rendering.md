## Rendering

Abstact Controller 封装 Action View
然后，给 Action Controller 和 Active Mailer 使用，而 Action Controller 根据自己情况又有自己的 render.

```
Action Controller & Active Mailer
                 |
                 V
          Abstact Controller
                 |
                 V
            Action View
```

我们自定义的 Controller#actions 里使用的 `render` 就是在这里定义的。

它其实是封装了 Action View 里的渲染相关内容，然后又被 Action Controller 所调用。

`render(*args, &block)` Controller 里的渲染。

封装了 Action View 里的 Template Renderer 和 Partial Renderer，提供 render 给 Action Controller, Action Mailer 渲染模板文件或局部模板。

真正的渲染工作并不是它做的，**它只是封装了 View 里的渲染方法**。所以，使用过程中有疑问的，可以查看 View 里对应 render 方法的文档。

除上述方法外，还有：

```
render_to_body
render_to_string

rendered_format

view_assigns
```

`view_assigns` 通过它可以查看 Controller 和 Mailer 传递给 View 的实例变量及其内容。
