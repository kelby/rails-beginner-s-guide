## view_context

**渲染相关里的上下文 - ActionView::Base 的实例对象。**

`view_context` 就是 ActionView::Base 的实例对象。
<br>
`_view_context_class` 默认就是 ActionView::Base

#### view_context 内容

```ruby
BlogsController.new.view_context
=> #<#<Class:0x007ff084a6ba90>:0x007ff07b745518
 @_assigns={"_routes"=>nil},
 @_config={},
 @_controller= BlogsController:0x007ff084a6bba8
 @_request=nil,
 @_routes=nil,
 @output_buffer=nil,
 @view_flow=#<ActionView::OutputFlow:0x007ff07b757010 @content={}>,
 @view_renderer= ActionView::Renderer:0x007ff07b745cc0
 @virtual_path=nil>
```

1) view_context.controller

```ruby
@_controller=
#<BlogsController:0x007ff084a6bba8
 @_action_has_layout=true,
 @_config={},
 @_headers={"Content-Type"=>"text/html"},
 @_lookup_context= ActionView::LookupContext:0x007ff07b7474a8
 @_prefixes=["blogs", "application"],
 @_request=nil,
 @_response=nil,
 @_routes=nil,
 @_status=200,
 @_view_context_class=#<Class:0x007ff084a6ba90>,
 @_view_renderer= ActionView::Renderer:0x007ff07b745cc0
```

2) view_context.view_renderer

```ruby
@view_renderer=
 #<ActionView::Renderer:0x007ff07b745cc0
  @lookup_context= ActionView::LookupContext:0x007ff07b7474a8
```

#### 类似概念 controller context

在 Controller 里，除了实例变量，我们还可以有其它方法传递内容给 View，两者方式类似。

```ruby
class BasicController < ActionController::Base
  # 1) 只引入对应模块
  include ActionView::Context

  # 2) 调用对应模块里的方法
  before_filter :_prepare_context

  def hello_world
    @value = "Hello"
  end

  protected
  # 3) 更改 view_context
  # 默认它是 ActionView::Base 的实例对象
  def view_context # STEP 3
    self
  end

  # 在 View 里可以调用此方法
  def __controller_method__
    "controller context!"
  end
end
```

```ruby
# app/views/basic/hello_world.html.erb
<%= @value %> from <%= __controller_method__ %>
```

因为 request/response 不是完整的，不能在浏览器里通过 url 访问，使用以下命令验证：

```
curl http://dev.shixian.com:3000/hello_world
```

返回：

```
# app/views/basic/hello_world.html.erb
Hello from controller context!
```

> Note: 本示例仅为了有助于理解 view_context 及理解 Rails 是如何渲染内容的，不可用于实例例子。

参考

[Design Principles
behind](http://cdn.oreillystatic.com/en/assets/1/event/59/SOLID%20Design%20Principles%20Behind%20The%20Rails%203%20Refactoring%20Presentation.pdf)
