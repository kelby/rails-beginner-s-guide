# Metal

一般模块名和同名目录都是有联系的，但 metal 不是，单指的是 metal.rb 这个文件，它和 metal/ 目录下的文件及内容没有关系。

---

<tt>ActionController::Metal</tt> is the simplest possible controller, providing a valid Rack interface without the additional niceties provided by <tt>ActionController::Base</tt>.

A sample metal controller might look like this:

```
  class HelloController < ActionController::Metal
    def index
      self.response_body = "Hello World!"
    end
  end
```

And then to route requests to your metal controller, you would add something like this to <tt>config/routes.rb</tt>:

```
  get 'hello', to: HelloController.action(:index)
```

The +action+ method returns a valid Rack application for the \Rails
router to dispatch to.

## Rendering Helpers

<tt>ActionController::Metal</tt> by default provides no utilities for rendering views, partials, or other responses aside from explicitly calling of <tt>response_body=</tt>, <tt>content_type=</tt>, and <tt>status=</tt>. To add the render helpers you're used to having in a normal controller, you can do the following:

```
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

```
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

## Others

不反对给 Rails 进行瘦身，但不要盲目，要清楚自己在做什么。

另外，要清楚的知道各个组件有什么用，添加是为了什么，去掉又会有什么影响。
