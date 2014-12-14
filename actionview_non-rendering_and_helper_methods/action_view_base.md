## Base

```ruby
module ActionView
  class Base
    attr_accessor :view_renderer
    attr_internal :config, :assigns

    delegate :lookup_context, :to => :view_renderer
    delegate :formats, :formats=, :locale, :locale=, :view_paths, :view_paths=, :to => :lookup_context

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
