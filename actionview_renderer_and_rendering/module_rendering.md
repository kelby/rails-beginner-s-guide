## Rendering

**承上启下的作用：**

```
          ActionController::Rendering
                    |
                    V
          AbstractController::Rendering
                    |
                    V
          ActionView::Rendering
                    |
                    V
          ActionView::Renderer
```

Action Controller 和 Abstract Controller 可以调用这里渲染相关的方法。

#### 实例方法：

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

**view_context 使用举例：**

通过 `view_context` 可以在 Controller 里调用 Helper 里的方法。

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

#### 类方法：

```
view_context_class
```

并且，ActionController 和 AbstactController 有同名方法，根据 Ruby 的继承规则，它们也可以调用这里的方法。

当然，还有其它方法，但不在此列举。
