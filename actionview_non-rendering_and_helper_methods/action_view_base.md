## Base

不同于其它几个组件，对外提供的接口是 Action View 模块本身，而不是 ActionView::Base 类。

```ruby
module ActionView
  class Base
    attr_accessor :view_renderer
    attr_internal :config, :assigns

    delegate :lookup_context, :to => :view_renderer
    delegate :formats, :formats=, :locale, :locale=, :view_paths, :view_paths=,
             :to => :lookup_context

    def initialize(context = nil, assigns = {}, controller = nil, formats = nil)
      @_config = ActiveSupport::InheritableOptions.new

      if context.is_a?(ActionView::Renderer)
        @view_renderer = context
      else
        lookup_context = context.is_a?(ActionView::LookupContext) ?
          context : ActionView::LookupContext.new(context)
        lookup_context.formats  = formats if formats
        lookup_context.prefixes = controller._prefixes if controller
        @view_renderer = ActionView::Renderer.new(lookup_context)
      end

      assign(assigns)
      assign_controller(controller)
      _prepare_context
    end
  end
end
```

因为，类和对象的概念，对于视图来说重要性没有那么大。

`view_context` 是 ActionView::Base 的实例对象。

```ruby
self.class.superclass
=> ActionView::Base
```

```ruby
Class.new(ActionView::Base) do
  if routes
    include routes.url_helpers(supports_path)
    include routes.mounted_helpers
  end

  if helpers
    include helpers
  end
end
```

在 ActionView::Rendering 里。

#### 怎么传递实例变量的？

有渲染才有传递实例变量这么一说。

```ruby
# action_view/rendering.rb
    def view_context
      view_context_class.new(view_renderer, view_assigns, self)
    end
```

```ruby
# abstract_controller/rendering.rb
def view_assigns
  # Rails 自带 & 自己管理的一些实例变量
      protected_vars = _protected_ivars
  # 此方法由 Ruby 提供，通过它我们可获取 Controller 里我们定义的一些实例变量
  variables      = instance_variables

  # ...
end
```

初始化 View 的时候，会指派实例变量：

```ruby
# action_view/base.rb
def assign(new_assigns) # :nodoc:
  @_assigns = new_assigns.each { |key, value| instance_variable_set("@#{key}", value) }
end
```
