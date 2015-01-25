## view_context

**渲染相关里的上下文 - ActionView::Base 的实例对象。**

`view_context` 就是 ActionView::Base 的实例对象。
<br>
`_view_context_class` 默认就是 ActionView::Base

**view_context**

```
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

```
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

```
@view_renderer=
 #<ActionView::Renderer:0x007ff07b745cc0
  @lookup_context= ActionView::LookupContext:0x007ff07b7474a8
```
