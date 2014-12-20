## Rendering

**承上启下的作用：**

```
          ActionController::Rendering
                    ^
                    |
          AbstractController::Rendering
                    ^
                    |
          ActionView::Rendering
                    ^
                    |
          ActionView::Renderer
```

**Action Controller 和 Abstract Controller 可以调用这里渲染相关的方法。**

**实例方法**

```
view_context
view_renderer

render_to_body

rendered_format

view_context_class
```

`view_renderer` 是 ActionView::Renderer 的实例对象。

`view_context` 是 ActionView::Base 的实例对象。

`render_to_body` 上面几个方法中，惟一的动词，会执行渲染程序。

**类方法**

```
view_context_class
```

并且，ActionController 和 AbstactController 有同名方法，根据 Ruby 的继承规则，它们也可以调用这里的方法。

---

此外，通过 view_context 可以在 Controller 里调用 Helper 里的方法。

举例一：

```ruby
module ApplicationHelper
  def fancy_helper(str)
    str.titleize
  end
end

class MyController < ApplicationController
   def index
     @title = view_context.fancy_helper "dogs are awesome"
   end
end
```

举例二：

```ruby
# app/controllers/posts_controller.rb
class PostsController < ActionController::Base
  def show
    # ...
    tags = view_context.generate_tags(@post)
    email_link = view_context.mail_to(@user.email)
    # ...
  end 
end

# app/helpers/posts_helper.rb
module PostsHelper
  def generate_tags(post)
    "Generate Tags"
  end
end
```

当然，还有其它方法，但不在此列举。

---

部分源代码：

```ruby
def view_context
  view_context_class.new(view_renderer, view_assigns, self)
end

def view_renderer
  @_view_renderer ||= ActionView::Renderer.new(lookup_context)
end

def view_context_class
  @_view_context_class ||= self.class.view_context_class
end

# view_context_class 等价于 ActionView::Base
def view_context_class
  @view_context_class ||= begin
    include_path_helpers = supports_path?
    routes  = respond_to?(:_routes)  && _routes
    helpers = respond_to?(:_helpers) && _helpers

    Class.new(ActionView::Base) do
      if routes
        include routes.url_helpers(include_path_helpers)
        include routes.mounted_helpers
      end

      if helpers
        include helpers
      end
    end
  end
end
```

---

它和它下面的 I18nProxy，都是从 Abstract Controller 转移到 Action View.
