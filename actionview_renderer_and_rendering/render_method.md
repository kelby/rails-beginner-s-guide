# render

```
spacer_template # 配合 collection 和 partical. 在局部模板之间穿插的内容。(为什么不把它们放到局部模板里，而是用这种奇怪的方式？可能是 partical 还被其它人使用，不能直接添加吧)
layout # 已编
partial # 渲染以 _ 开头的局部模板
locals # 已编
formats
object # 每一个局部模板都有一个和它同名的变量名可用，使用 object 可设置这个变量的值(类似 locals，但这里只有一个变量，且变量名固定)
as # 配合 collection. 渲染一个集合后，默认同名的变量名可能不同，我们需要指定，使用 as 可设置这个变量(类似 object)
collection # 集合里的每一个元素都要渲染同一个局部模板，所以可以传递一个集合给局部模板。作用和 collection.each 之后再 render 一样的

layout # 默认都是使用当前布局，你可以更改或不使用布局
locals # 传递变量给局部模板，以 Hash 的形式编写，可以传递多个变量，名字自取
body # 只返回 content，而不关心 content type. 区别于 :plain 和 :html (实际渲染的是 text)
text # 渲染纯文本内容
plain # 仅仅为了返回很简单的数据。这里没有 HTML 状态和 layout (实际渲染的是 text, 没有 layout)
html # 直接渲染代码，默认使用 HTML 格式。不好的实践(实际渲染的是 html, 没有 layout)
file # 明确指定渲染哪个文件，一般这个 key 可省，传 value 即可。默认不使用 layout (实际是渲染 template)
inline # 直接渲染代码，默认使用 ERB 格式。不好的实践 (不使用 layout)
template # 明确指定渲染哪个模板，一般这个 key 可省，传 value 即可
prefixes

[:body, :text, :plain, :html]

update (注意区别于 js，这里是 RJS)
Rendering inline JavaScriptGenerator page updates

In addition to rendering JavaScriptGenerator page updates with Ajax in RJS templates (see ActionView::Base for details), you can also pass the :update parameter to render, along with a block, to render page updates inline.

  render :update do |page|
    page.replace_html  'user_list', :partial => 'user', :collection => @users
    page.visual_effect :highlight, 'user_list'
  end

nothing # 有时候我们不希望渲染任何东西(渲染的是 text)
status # 对应着 HTTP status code
content_type # html, json, xml 都是可以自动识别 content type 的，其它格式可能你需要指定
location # 对应着 HTTP Location header

action # 渲染某个 action
type # 配合 inline，可以渲染其它类型的代码(默认是 ERB)
```

    * <tt>:partial</tt> - See <tt>ActionView::PartialRenderer</tt>.
    * <tt>:file</tt> - Renders an explicit template file (this used to be the old default), add :locals to pass in those.
    * <tt>:inline</tt> - Renders an inline template similar to how it's done in the controller.
    * <tt>:text</tt> - Renders the text passed in out.
    * <tt>:plain</tt> - Renders the text passed in out. Setting the content
      type as <tt>text/plain</tt>.
    * <tt>:html</tt> - Renders the HTML safe string passed in out, otherwise
      performs HTML escape on the string first. Setting the content type as
      <tt>text/html</tt>.
    * <tt>:body</tt> - Renders the text passed in, and inherits the content
      type of <tt>text/html</tt> from <tt>ActionDispatch::Response</tt>
      object.

```ruby
ActionController::Renderers::RENDERERS
 => #<Set: {:json, # 返回 json 格式的数据(使用了 to_json)
 :js, # 直接渲染代码，默认是 JavaScript 格式
 :xml # 返回 xml 格式的数据(使用了 to_xml)
 }> 
 ```

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

API 说

```
:partial - See ActionView::PartialRenderer.

:file - Renders an explicit template file (this used to be the old default), add :locals to pass in those.

:inline - Renders an inline template similar to how it's done in the controller.

:text - Renders the text passed in out.

:plain - Renders the text passed in out. Setting the content type as text/plain.

:html - Renders the HTML safe string passed in out, otherwise performs HTML escape on the string first. Setting the content type as text/html.

:body - Renders the text passed in, and inherits the content type of text/html from ActionDispatch::Response object.
```

示例

```
Action:

# The default. Does not need to be specified 
# in a controller method called "some_action"
render :action => 'some_action'   
render :action => 'another_action', :layout => false
render :action => 'some_action', :layout => 'another_layout'
Partial:

Partials are stored in files called "_subformname" ( _error, _subform, _listitem)

render :partial => 'subform'
render :partial => 'error', :status => 500
render :partial => 'subform', :locals => 
                   { :variable => @other_variable }
render :partial => 'listitem', :collection => @list
render :partial => 'listitem', :collection => @list, 
                   :spacer_template => 'list_divider'
Template:

Like rendering an action, but finds the template based on the template root (app/views)

# renders app/views/weblog/show
render :template => 'weblog/show' 
File:

render :file => '/path/to/some/file.rhtml'
render :file => '/path/to/some/filenotfound.rhtml', 
                    status => 404, :layout => true
Text:

render :text => "Hello World"
render :text => "This is an error", :status => 500
render :text => "Let's use a layout", :layout => true
render :text => 'Specific layout', :layout => 'special'
Inline Template:

Uses ERb to render the "miniature" template

render :inline => "<%= 'hello , ' * 3 + 'again' %>"
render :inline => "<%= 'hello ' + name %>", 
                   :locals => { :name => "david" }
Nothing:

render :nothing
render :nothing, :status => 403    # forbidden
RJS:

def refresh
  render :update do |page|
    page.replace_html  'user_list', :partial => 'user', 
                                    :collection => @users
    page.visual_effect :highlight, 'user_list'
  end
end
Change the content-type:

render :action => "atom.rxml", 
       :content_type => "application/atom+xml"
```

渲染其它格式

```
ActionController.add_renderer :json
```

Particl Render

```
<%= render partial: "account" %>
<%= render partial: "account", locals: { account: @buyer } %>
<%= render partial: "account", object: @buyer %>
<%= render partial: "account", object: @buyer, as: 'user' %>
<%= render partial: "ad", collection: @advertisements %>
```


参考：

[method render](http://apidock.com/rails/ActionController/Base/render)  
[Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html)  
[Action View Partials](http://api.rubyonrails.org/classes/ActionView/PartialRenderer.html)  
[Rails View Rendering using Naming Convention](http://jonathanhui.com/ruby-rails-3-view)  
[Render Options in Rails 3](https://blog.engineyard.com/2010/render-options-in-rails-3/)  
[Ruby on Rails - Render](http://www.tutorialspoint.com/ruby-on-rails/rails-render.htm)


You invoked render but did not give any of :partial, :template, :inline, :file, :plain, :text or :body option.

Controller 里默认渲染的是 template
走的路是 ActionController::Rendering -> AbstractController::Rendering -> ActionView::Rendering -> ActionView::Renderer#render

View 里默认渲染的是 partial
这几个选项，不能混合使用
走的路是 ActionView::Helpers::RenderingHelper#render -> ActionView::Renderer

通过上面的路径和特别指出的两个 render 方法里面的逻辑，不难看出为什么可以默认渲染 template 或 partial.

只渲染 json，只得到数据

只渲染 js 报错，安全隐患

Controller 里可以指定渲染 partial，但 View 里不可指定渲染 template
