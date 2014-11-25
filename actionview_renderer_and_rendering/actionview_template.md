## Template

```ruby
ERBHandler = ActionView::Template::Handlers::ERB.new

def new_template(body = "<%= hello %>", details = { format: :html })
  ActionView::Template.new(body, "hello template", details.fetch(:handler) { ERBHandler }, {:virtual_path => "hello"}.merge!(details))
end

@template = new_template
@template = new_template("<%= apostrophe %>")
@template = new_template("<%= apostrophe %> <%== apostrophe %>", format: :text)
@template = new_template("<%= hello %>", :handler => ActionView::Template::Handlers::Raw.new)

# 看看新建模板，要求是什么
def initialize(source, identifier, handler, details)
  format = details[:format] || (handler.default_format if handler.respond_to?(:default_format))

  @source            = source
  @identifier        = identifier
  @handler           = handler
  @compiled          = false
  @original_encoding = nil
  @locals            = details[:locals] || []
  @virtual_path      = details[:virtual_path]
  @updated_at        = details[:updated_at] || Time.now
  @formats           = Array(format).map { |f| f.respond_to?(:ref) ? f.ref : f  }
  @variants          = [details[:variant]]
  @compile_mutex     = Mutex.new
end
```
