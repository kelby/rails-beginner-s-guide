## Metal 使用举例

### 原生的 Metal

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣


在 Rails 里 metal 也属于 middleware，我们可以这么用：

```ruby
# config/routes.rb
  get 'hello' => 'hello#index'
  ...
```

```ruby
# app/controllers/hello_controller.rb
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

然后浏览器访问：http://localhost:3000/hello 可以获取刚才的内容。

日志

    Started GET "/hello" for 127.0.0.1 at 2014-04-27 08:57:07 +0800

改为

```
Started GET "/hello" for 127.0.0.1 at 2014-04-27 09:04:49 +0800
Processing by HelloController#index as HTML
Completed 200 OK in 1ms (ActiveRecord: 0.0ms)
```

### 加入 Rendering 模块

默认 <tt>ActionController::Metal</tt> 是没有提供渲染视图、模板或其它需要明确调用 <tt>response_body=</tt>, <tt>content_type=</tt>, 和 <tt>status=</tt> 的方法。如果你需要这些，可以引入它们：

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

### 加入 Redirection 模块

想使用重定向相关代码，你也需要引入它们：

```ruby
class HelloController < ActionController::Metal
  include ActionController::Redirecting
  include Rails.application.routes.url_helpers

  def index
    redirect_to root_url
  end
end
```

### 加入其它模块

参考 <tt>ActionController::Base</tt> 引入其它模块，以达到目的。
