# Metal

metal 里的 middleware_stack 循环执行，metal 之外的东西是附属品。

一般模块名和同名目录都是有联系的，但 metal 不是，单指的是 metal.rb 这个文件，它和 metal/ 目录下的文件及内容没有关系。

---

MVC 里的 C 可以做得很精简，ActionController::Metal 就是例子。除了提供一个有效的 Rack 接口外，它几乎没有任何其它功能。

举个例子:

```ruby
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

在路由里添加相应代码，将请求转发到刚才的 HelloController#index 进行处理:

```ruby
# in your config/routes.rb
get 'hello', to: HelloController.action(:index)
```

The +action+ method returns a valid Rack application for the \Rails router to dispatch to.

## Rendering Helpers

<tt>ActionController::Metal</tt> by default provides no utilities for rendering views, partials, or other responses aside from explicitly calling of <tt>response_body=</tt>, <tt>content_type=</tt>, and <tt>status=</tt>. To add the render helpers you're used to having in a normal controller, you can do the following:

```ruby
class HelloController < ActionController::Metal
  include AbstractController::Rendering
  include ActionView::Layouts
  append_view_path "#{Rails.root}/app/views"

  def index
    render "hello/index"
  end
end
```

## Redirection Helpers

To add redirection helpers to your metal controller, do the following:

```ruby
class HelloController < ActionController::Metal
  include ActionController::Redirecting
  include Rails.application.routes.url_helpers

  def index
    redirect_to root_url
  end
end
```

## Other Helpers

You can refer to the modules included in <tt>ActionController::Base</tt> to see other features you can bring into your metal controller.

## MiddlewareStack

middleware 并不总是需要项目级别的，它也可以精确到某个 Controller，甚至是 action.

Extend ActionDispatch middleware stack to make it aware of options
allowing the following syntax in controllers:

```ruby
class PostsController < ApplicationController
  use AuthenticationMiddleware, except: [:index, :show]
end
```

`self.use` Pushes the given Rack middleware and its arguments to the bottom of the middleware stack.

Metal 里的代码：

```ruby
# Returns a Rack endpoint for the given action name.
def self.action(name, klass = ActionDispatch::Request)
  if middleware_stack.any?
    middleware_stack.build(name) do |env|
      new.dispatch(name, klass.new(env))
    end
  else
    lambda { |env| new.dispatch(name, klass.new(env)) }
  end
end
```

## Others

不反对给 Rails 进行瘦身，但不要盲目，要清楚自己在做什么。

另外，要清楚的知道各个组件有什么用，添加是为了什么，去掉又会有什么影响。

---

總而言之，如果你在 Rails3 中不需要全部的 Controller 的功能，想要盡量拉高效能，有幾種推薦作法：

* 寫成 Rack Middleware，然後在 config/application.rb 中插入 config.middleware.use YourMiddleWare
* 寫成 Rack App，在 config.route.rb 中將某個對應的網址指到這個 Rack App
* 繼承自 ActionController::Metal 實作一個 Controller，其中的 action 也是一個 Rack App

其中的差異就在於後兩者會在 Rails Route 之後（好處是統一由 route.rb 管理 URL 路徑），如果繼承自 ActionController::Metal 可以有彈性獲得更多 Controller 功能。原則上，我想我會推薦 ActionController::Metal，寫起來最為簡單，一致性跟維護性較高。

---

为什么能够连续调用，原因：

你看每个 Rack Middleware 的 call 函数的最后一行，是不是都是 @app.call(env)
这说明，它在调用下一个 middleware 啊

它们是一条封闭的链接，一直走下去，最后又会回到开头处，并且中间只要有一处断了，那整条链子就都 走不通！

顺序是：默认是按 use 的顺序走下去，但 use 时你也是可以指定的。

> Note: @app 和 env 一直在变，但又一直没变。

---

