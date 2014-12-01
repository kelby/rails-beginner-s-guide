## 渲染及相关概念

[Design Principles behind - Oreillystatic](http://cdn.oreillystatic.com/en/assets/1/event/59/SOLID%20Design%20Principles%20Behind%20The%20Rails%203%20Refactoring%20Presentation.pdf)

[Render template if exists in Rails](https://coderwall.com/p/ftbmsa)

--------------

文件路径
文件名，包括类型(也就是格式)
文件里的局部变量

template == 文件路径 + 文件名(包括类型) ？

```
每一次渲染过后都有缓存的！
渲染其实质就是 ActionView::Base.new(view_paths, {}).render(*args)

有缓存：view_paths = ActionController::Base.view_paths

无缓存：
path = ActionView::FileSystemResolver.new(FIXTURE_LOAD_PATH)
view_paths = ActionView::PathSet.new([path])
```

上下文包含格式
上下文的 view_paths 是 PathSet
上下文的本地化
上下文的变种
上下文有缓存

模板包含内容
源内容，格式

template = @lookup_context.find("hello", %w(test))

@view = ActionView::Base.new(paths, @assigns)
@controller_view = TestController.new.view_context

@view
有 render
有 lookup_context
有 locale

@controller_view
有 render 方法

pattern = ":prefix/{:formats/,}:action{.:formats,}{.:handlers,}"
@resolver = ActionView::FileSystemResolver.new(path, pattern)
@resolver
有 find_all

@view_renderer = ActionView::Renderer.new(lookup_context)

**注意，这很重要**

```ruby
def render_partial(context, options, &block) #:nodoc:
  PartialRenderer.new(@lookup_context).render(context, options, block)
end
```

和 `ERB.new( template ).result( binder )` 是不是很像

模板由上下文而来

```ruby
def find_template(path, locals)
  prefixes = path.include?(?/) ? [] : @lookup_context.prefixes
  @lookup_context.find_template(path, prefixes, true, locals, @details)
end
```

```ruby
pry(main)> BlogsController.new.view_context
=> #<#<Class:0x007ff084a6ba90>:0x007ff07b745518
 @_assigns={"_routes"=>nil},
 @_config={},
 @_controller=
  #<BlogsController:0x007ff084a6bba8
   @_action_has_layout=true,
   @_config={},
   @_headers={"Content-Type"=>"text/html"},
   @_lookup_context=
    #<ActionView::LookupContext:0x007ff07b7474a8
     @cache=true,
     @details=
      {:locale=>[:en],
       :formats=>
        [:html,
         :text,
         :js,
         :css,
         :ics,
         :csv,
         :png,
         :jpeg,
         :gif,
         :bmp,
         :tiff,
         :mpeg,
         :xml,
         :rss,
         :atom,
         :yaml,
         :multipart_form,
         :url_encoded_form,
         :json,
         :pdf,
         :zip,
         :xls],
       :handlers=>[:erb, :builder, :coffee, :slim]},
     @details_key=nil,
     @prefixes=["blogs", "application"],
     @rendered_format=nil,
     @skip_default_locale=false,
     @view_paths=
      #<ActionView::PathSet:0x007ff07b7473b8
       @paths=
        [#<ActionView::OptimizedFileSystemResolver:0x007ff08158b9b0
          @cached={},
          @path="/Users/kelby/origin/console/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff0815912c0
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/switch_user-0.9.4/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081592c38
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/bundler/gems/bootstrap-kaminari-views-55f8c238cd99/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081593de0
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/kaminari-0.15.1/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">]>>,
   @_prefixes=["blogs", "application"],
   @_request=nil,
   @_response=nil,
   @_routes=nil,
   @_status=200,
   @_view_context_class=#<Class:0x007ff084a6ba90>,
   @_view_renderer=
    #<ActionView::Renderer:0x007ff07b745cc0
     @lookup_context=
      #<ActionView::LookupContext:0x007ff07b7474a8
       @cache=true,
       @details=
        {:locale=>[:en],
         :formats=>
          [:html,
           :text,
           :js,
           :css,
           :ics,
           :csv,
           :png,
           :jpeg,
           :gif,
           :bmp,
           :tiff,
           :mpeg,
           :xml,
           :rss,
           :atom,
           :yaml,
           :multipart_form,
           :url_encoded_form,
           :json,
           :pdf,
           :zip,
           :xls],
         :handlers=>[:erb, :builder, :coffee, :slim]},
       @details_key=nil,
       @prefixes=["blogs", "application"],
       @rendered_format=nil,
       @skip_default_locale=false,
       @view_paths=
        #<ActionView::PathSet:0x007ff07b7473b8
         @paths=
          [#<ActionView::OptimizedFileSystemResolver:0x007ff08158b9b0
            @cached={},
            @path="/Users/kelby/origin/console/app/views",
            @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
           #<ActionView::OptimizedFileSystemResolver:0x007ff0815912c0
            @cached={},
            @path=
             "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/switch_user-0.9.4/app/views",
            @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
           #<ActionView::OptimizedFileSystemResolver:0x007ff081592c38
            @cached={},
            @path=
             "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/bundler/gems/bootstrap-kaminari-views-55f8c238cd99/app/views",
            @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
           #<ActionView::OptimizedFileSystemResolver:0x007ff081593de0
            @cached={},
            @path=
             "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/kaminari-0.15.1/app/views",
            @pattern=
             ":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">]>>>>,
 @_request=nil,
 @_routes=nil,
 @output_buffer=nil,
 @view_flow=#<ActionView::OutputFlow:0x007ff07b757010 @content={}>,
 @view_renderer=
  #<ActionView::Renderer:0x007ff07b745cc0
   @lookup_context=
    #<ActionView::LookupContext:0x007ff07b7474a8
     @cache=true,
     @details=
      {:locale=>[:en],
       :formats=>
        [:html,
         :text,
         :js,
         :css,
         :ics,
         :csv,
         :png,
         :jpeg,
         :gif,
         :bmp,
         :tiff,
         :mpeg,
         :xml,
         :rss,
         :atom,
         :yaml,
         :multipart_form,
         :url_encoded_form,
         :json,
         :pdf,
         :zip,
         :xls],
       :handlers=>[:erb, :builder, :coffee, :slim]},
     @details_key=nil,
     @prefixes=["blogs", "application"],
     @rendered_format=nil,
     @skip_default_locale=false,
     @view_paths=
      #<ActionView::PathSet:0x007ff07b7473b8
       @paths=
        [#<ActionView::OptimizedFileSystemResolver:0x007ff08158b9b0
          @cached={},
          @path="/Users/kelby/origin/console/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff0815912c0
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/switch_user-0.9.4/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081592c38
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/bundler/gems/bootstrap-kaminari-views-55f8c238cd99/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081593de0
         :ics,
         :csv,
         :png,
         :jpeg,
         :gif,
         :bmp,
         :tiff,
         :mpeg,
         :xml,
         :rss,
         :atom,
         :yaml,
         :multipart_form,
         :url_encoded_form,
         :json,
         :pdf,
         :zip,
         :xls],
       :handlers=>[:erb, :builder, :coffee, :slim]},
     @details_key=nil,
     @prefixes=["blogs", "application"],
     @rendered_format=nil,
     @skip_default_locale=false,
     @view_paths=
      #<ActionView::PathSet:0x007ff07b7473b8
       @paths=
        [#<ActionView::OptimizedFileSystemResolver:0x007ff08158b9b0
          @cached={},
          @path="/Users/kelby/origin/console/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff0815912c0
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/switch_user-0.9.4/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081592c38
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/bundler/gems/bootstrap-kaminari-views-55f8c238cd99/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,
         #<ActionView::OptimizedFileSystemResolver:0x007ff081593de0
          @cached={},
          @path=
           "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/kaminari-0.15.1/app/views",
          @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">]>>>,
 @virtual_path=nil>
```

-----------

[Rendering Partials in Rails - Single Responsibility Principle](http://bl.ocks.org/wangjohn/5418350)

[Rails 3 at RailsConf 2011](http://blog.endpoint.com/2011/05/rails-3-at-railsconf-2011.html)

- View Path: holds resolvers
- Resolver: finds template. The resolvers abstraction no longer restricts templates to the filesystem (can be anywhere in the app, web service, or even database) which simplifies testing and therefore improves maintainability.
- Lookup Context: tracks details, lookup context object is passed to the view.
- AV::Renderer: renders templates
- ActionView::Base: renders context

上面说的是 View 里的渲染，那么在 Controller 里的一样吗？

```ruby
# abstract_controller/rendering.rb
def render(*args, &block)
  options = _normalize_render(*args, &block)
  self.response_body = render_to_body(options)
end
```

下一步

```ruby
def _render_template(options) #:nodoc:
  lookup_context.rendered_format = nil if options[:formats]
  view_renderer.render(view_context, options)
end
```

再下一步

```ruby
# abstract_controller/view_paths.rb
def lookup_context
  @_lookup_context ||=
    ActionView::LookupContext.new(self.class._view_paths, details_for_lookup, _prefixes)
end
```

正式进入 ActionView 的世界！

```ruby
def view_renderer
  @_view_renderer ||= ActionView::Renderer.new(lookup_context)
end
```

串起来就是：
`ActionView::Renderer.new(ActionView::LookupContext.new(self.class._view_paths, details_for_lookup, _prefixes)).render(view_context, options)`

这上面的 `render`

```ruby
def render(context, options)
  if options.key?(:partial)
    render_partial(context, options)
  else
    render_template(context, options)
  end
end
```

```ruby
# Direct accessor to template rendering.
def render_template(context, options) #:nodoc:
  _template_renderer.render(context, options)
end

# Direct access to partial rendering.
def render_partial(context, options, &block) #:nodoc:
  _partial_renderer.render(context, options, block)
end

private

def _template_renderer #:nodoc:
  @_template_renderer ||= TemplateRenderer.new(@lookup_context)
end

def _partial_renderer #:nodoc:
  @_partial_renderer ||= PartialRenderer.new(@lookup_context)
end
```

lookup_context 就是 binding 作用！！！

`view_renderer.render(view_context, options)`

这里的 render 及 view_context 是什么？为什么它又反过来需要 view_renderer

```ruby
def view_context
  view_context_class.new(view_renderer, view_assigns, self)
end

def view_context_class
  @view_context_class ||= begin
    routes  = _routes  if respond_to?(:_routes)
    helpers = _helpers if respond_to?(:_helpers)
    ActionView::Base.prepare(routes, helpers)
  end
end

# This method receives routes and helpers from the controller
# and return a subclass ready to be used as view context.
def prepare(routes, helpers) #:nodoc:
  Class.new(self) do
    if routes
      include routes.url_helpers
      include routes.mounted_helpers
    end

    if helpers
      include helpers
      self.helpers = helpers
    end
  end
end
```

**注意，渲染的结果是**

```ruby
def render(*args, &block)
  options = _normalize_render(*args, &block)
  self.response_body = render_to_body(options)
end
```

和

```ruby
class HelloController < Metial
  def index
    self.response_body = "hello"
  end
end
```

本质是一样的。

> **总结：** 根据上下文，找到模板，然后渲染处理它。

让我们模拟一下。

```ruby
pry(#<BlogsController>)> self.view_paths
=> #<ActionView::PathSet:0x007f9a0dcc2938
 @paths=

  [#<ActionView::OptimizedFileSystemResolver:0x007f9a0fc149d0
    @cached={},
    @path="/Users/kelby/origin/console/app/views",
    @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,

   #<ActionView::OptimizedFileSystemResolver:0x007f9a0fc16208
    @cached={},
    @path=
     "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/switch_user-0.9.4/app/views",
    @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,

   #<ActionView::OptimizedFileSystemResolver:0x007f9a0fc17608
    @cached={},
    @path=
     "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/bundler/gems/bootstrap-kaminari-views-55f8c238cd99/app/views",
    @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">,

   #<ActionView::OptimizedFileSystemResolver:0x007f9a0fc17ea0
    @cached={},
    @path=
     "/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/kaminari-0.15.1/app/views",
    @pattern=":prefix/:action{.:locale,}{.:formats,}{.:handlers,}">]>
```


模板有这些易于理解的属性：

```ruby
attr_accessor :locals, :formats, :variants, :virtual_path

attr_reader :source, :identifier, :handler, :original_encoding, :updated_at
```

主要涉及：

view_paths.rb<br>
path_set.rb<br>
lookup_context.rb<br>
template.rb 及其目录(error, handlers, html, resolver, text, types)<br>
rendering.rb<br>
renderer 目录<br>
base.rb<br>
又引出了不太重要的 context.rb, flows.rb
