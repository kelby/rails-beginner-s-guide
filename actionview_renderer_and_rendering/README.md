# ActionView 渲染相关(renderer & rendering)

## View Paths

- lookup_context

LookupContext 查找上下文，很重要概念。它携带着很多信息，如：view paths 和 details，查找模板需要这些东西。
当然，对我们平常使用没有影响，知道有这么回事即可。

- _view_paths

及相关操作，如：prepend_view_path、append_view_path
这个在某些场合还是用得到的。

`append_view_path(path)`<br>
追加路径到当前 Controller 所在的 view paths 里。当我们的自己创建或者使用的 gem 引入的文件目录结构和Rails默认不一样，但且希望渲染到视图里的模板(局部模板)，就会报错，此方法可以帮助我们。

`prepend_view_path` 和 append_view_path 方法类似

## Template

模板对象及其信息。

```ruby
ERBHandler = ActionView::Template::Handlers::ERB.new

body = "<%= hello %>"
details = { format: :html }

new_template = ActionView::Template.new(body, "hello template", details.fetch(:handler) { ERBHandler }, {:virtual_path => "hello"}.merge!(details))
```

当然，这些我们平时都接触不到，知道有这么回事即可。

## Rendering

概念 view_context & view_renderer

```ruby
def view_context
  view_context_class.new(view_renderer, view_assigns, self)
end

def view_renderer
  @_view_renderer ||= ActionView::Renderer.new(lookup_context)
end

def view_context_class
  @_view_context_class ||= self.class.view_context_class
end

# view_context_class 等价于 ActionView::Base
def view_context_class
  @view_context_class ||= begin
    include_path_helpers = supports_path?
    routes  = respond_to?(:_routes)  && _routes
    helpers = respond_to?(:_helpers) && _helpers

    Class.new(ActionView::Base) do
      if routes
        include routes.url_helpers(include_path_helpers)
        include routes.mounted_helpers
      end

      if helpers
        include helpers
      end
    end
  end
end
```

## 其它

搜索了一下，API 里 `render` 同名方法有 8 个，它们分别代表什么意思？

而 ActionView 就有 6 个 render 方法(其中 2 个和测试有关)，分别在：

- Helpers::RenderingHelper - render(options = {}, locals = {}, &block)

要使用这个模块，你需要实现 `view_renderer` 方法，这个方法返回一个 ActionView::Renderer 对象。

- PartialRenderer - render(context, options, block)

用于渲染局部模板。

- Renderer - render(context, options)

ActionView 和 ActionController 渲染的主要入口。

- Template - render(view, locals, buffer=nil, &block)

剩下两个在 AbstractController::Rendering 和 ActionController::Instrumentation 里。
