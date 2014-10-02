# Metal 使用举例

## Rendering Helpers

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

## Redirection Helpers

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

## Other Helpers

参考 <tt>ActionController::Base</tt> 引入其它模块，以达到目的。
