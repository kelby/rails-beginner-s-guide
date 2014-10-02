- 服务于 ActionController 和 ActionMailer
- 将站场从 ActionDispatch 转移到 Controller 或 Mailer

AbstractController 无论是它自己定义的方法，还是封装 ActionView 和 ActionDispatch 得到的方法，最终都提供给 ActionController 和 ActionMailer 使用。  
这些方法(或模块)包含但不限于渲染(模板或局部模板)、Helper相关、回调、Mime、UrlFor 等。

AbstractController 起到了承上启下的作用(所以，有时候它的代码会让你觉得很迷惑)，及 ActionDispatch 到 ActionController 转换，并且也实现了一些重要的方法，如：Callbacks、Helpers.

下面只挑选部分模块来讲解，其它的模块，可以参考"源码剖析"。

## Helpers

不是指辅助方法！

`helper(*args, &block)` 把 Helper 方法变成 Controller 方法。

参数类型，可分为 3 类：String、Symbol，Module，block。这些参数还可以混合使用。

- 当参数是模块名时，直接 include 此模块 (没有 requires )

```
helper FooHelper # => includes FooHelper
```

- 而当参数是字符串或符号时，会根据约定 require 相关文件，并 include 相关模块。

```
helper :foo             # => requires 'foo_helper' and includes FooHelper
helper 'resources/foo'  # => requires 'resources/foo_helper' and includes Resources::FooHelper
```

此外，helper 可以接受并处理一个代码块。

```ruby
# One line
helper { def hello() "Hello, world!" end }

# Multi-line
helper do
  def foo(bar)
    "#{bar} is the very best"
  end
end
```

最后要说的提，上述说的参数类型可以混合使用，你可以同时传递符号、字符串、模块和代码块给 helper 方法。

```ruby
helper(:three, BlindHelper) { def mice() 'mice' end }
```

具体实现(使用说明完全是多余的)：

```ruby
def helper(*args, &block)
  # String、Symbol，Module 先格式化
  modules_for_helpers(args).each do |mod|
    add_template_helper(mod) # => _helpers.module_eval { include mod }
  end

  # block
  _helpers.module_eval(&block) if block_given?
end
```

`helper_method(*meths)` 把 Controller 方法变成 Helper 方法。

如下文举例，将 current_user 方法，可以在视图里使用：

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user, :logged_in?

  def current_user
    @current_user ||= User.find_by(id: session[:user])
  end

  def logged_in?
    current_user != nil
  end
end
```

在视图里:

```ruby
<% if logged_in? -%>Welcome, <%= current_user.name %><% end -%>
```

具体实现(以元编程的形式定义定义同名方法，然后 send 调用原helper方法)

```ruby
meths.each do |meth|
  _helpers.class_eval <<-ruby_eval, __FILE__, __LINE__ + 1
    def #{meth}(*args, &blk)                               # def current_user(*args, &blk)
      controller.send(%(#{meth}), *args, &blk)             #   controller.send(:current_user, *args, &blk)
    end                                                    # end
  ruby_eval
end
```

helper 和 helper_method 可以简单理解为一对作用相反的操作。

## Callbacks

```ruby
before_action(alias append_before_action), prepend_before_action, skip_before_action

after_action(alias append_after_action),   prepend_after_action,  skip_after_action

around_action(alias append_around_action), prepend_around_action, skip_around_action

skip_action_callback(alias skip_filter)
```

具体实现：调用 ActiveSupport::Callbacks 的 `set_callback` 和 `skip_callback` 等方法。

上面 before, after, around 的 append, prepend, skip 类似，明白一个其它的可以贯通。
skip_action_callback 意味着 skip: before, after, around

## Rendering

`render(*args, &block)` Controller 里的渲染。

封装了 ActionView 里的 TemplateRenderer 和 PartialRenderer，提供 render 给 ActionController, ActionMailer 渲染模板文件或局部模板。

真正的渲染工作并不是它完成，它只是封装了 View 里的渲染方法。所以，使用过程中有疑问的，可以查看 View 里对应 render 方法的文档。

1) 不想渲染任何东西，可以使用：

```ruby
render nothing: true
```

此时，默认 status = 200, 你也可以手动指定状态码。

2) 不想渲染任何东西，还可以使用方法：

`head 

3) 不想渲染，只想查看结果可以使用 `render_to_string`  
传递给它的参数和 render 一样，但它始终返回一个字符串。

4) 一些常用可选参数

明确指定要渲染的模板，用 template

```
render template: "products/show"
```

明确指定要渲染的文件，用 file

```
render file: "/u/apps/warehouse_app/current/app/views/products/show"
```

沉浸文件，默认是没有 layout 的，如果需要，你可以手动指定 `layout: true`

5) 不用模板，但效果类似，用 inline

```
render inline: "<% products.each do |p| %><p><%= p.name %></p><% end %>"
```

默认后面的代码用 ERB 解析，如果需要，你可以手动指定 `type: :builder`

inline 违背了 MVC 模式，实践起来并不友好，不推荐使用。

6) 渲染纯文本，用 plain

```
render plain: "OK"
```

7) 不用模板，但效果类似，用 html

```
render html: "<strong>Not Found</strong>".html_safe
```

和 inline 一样，这违背了 MVC 模式，实践起来并不友好，不推荐使用。

8) 渲染返回 json

```
render json: @product
```

这里的数据会自动转换成 json 格式，不需要调用 to_json

9) 渲染返回 xml

```
render xml: @product
```

这里的数据会自动转换成 xml 格式，不需要调用 to_xml

10) 渲染返回 js

```
render js: "alert('Hello Rails');"
```

11) 渲染，但不指定类型，用 body

```
render body: "raw"
```

## Base

ActionDispatch -> Metal -> AbstractController -> ActionController 请求是如何转变的？部分答案在这 ...

有方法：

```
abstract!, action_methods, available_action?
clear_action_methods!, controller_path
hidden_actions
internal_methods
method_added
process
supports_path?
```

下面只讲解一些可能有用的方法。

`controller_path()` 返回当前 Controller 所在的路径(包括目录、文件名)。例如，MyApp::MyPostsController 返回"my_app/my_posts"。

`action_methods()` 返回当前 Class 所包含的 action，默认等同于 public_instance_methods. 这里的 Class 可以是 Controller，也可以是 Mailer. 对于 AbstractController 来说，它们都是 Base 的子类，概念一样。

那么，如何获取当前程序所有的 Controller 和 action ?

获取所有的 Controller:

```ruby
ApplicationController.descendants
```

开发环境默认是延迟加载的，所以获取的只是当前 Controller，为了达到目的，需要先运行以下命令：

```ruby
Rails.application.eager_load!
```

生产环境，不必运行。

已经获取了所有的 Controller，那么获取某个 Controller 所有的 action，用刚才提到的 `action_methods` 即可：

```ruby
PostsController.action_methods
```

## Collector

我们的响应格式。

```ruby
# in your Controller
respond_to :html, :xml, :json

# in your actions
respond_to do |format|
  format.html
  format.xml { render xml: @people }
end
```

目前 Rails 支持的 MIME(多用途互联网邮件扩展)。

>html
text
js
css
ics
csv
vcf
>
png
jpeg
gif
bmp
tiff
>
mpeg
>
xml
rss
atom
yaml
>
multipart_form
url_encoded_form
>
json
>
pdf
zip
