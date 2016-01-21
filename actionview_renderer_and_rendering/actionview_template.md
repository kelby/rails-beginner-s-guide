## Template 内容

**模板对象的内容，及其所携带的信息。**

```ruby
ERBHandler = ActionView::Template::Handlers::ERB.new

body = "<%= hello %>"
details = { format: :html }

def new_template(body = "<%= hello %>", details = { format: :html })
  ActionView::Template.new(body, "hello template",
                           details.fetch(:handler) { ERBHandler },
                           {:virtual_path => "hello"}.merge!(details))
end

@template = new_template
@template = new_template("<%= apostrophe %>")
@template = new_template("<%= apostrophe %> <%== apostrophe %>", format: :text)
@template = new_template("<%= hello %>",
                         :handler => ActionView::Template::Handlers::Raw.new)
```

当然，这些我们平时都接触不到，知道有这么回事即可。

```ruby
attr_accessor :locals, :formats, :variants, :virtual_path

attr_reader :source, :identifier, :handler, :original_encoding, :updated_at

# 看看新建模板，要求是什么
def initialize(source, identifier, handler, details)
  if handler.respond_to?(:default_format))
    format = details[:format] || (handler.default_format
  end

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
