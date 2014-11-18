## Rendering

```ruby
render_to_body
rendered_format
view_context
view_context_class
view_renderer
```

`view_renderer` 是 ActionView::Renderer 的实例对象。

`view_context` 是 ActionView::Base 的实例对象。

并且，ActionController 和 AbstactController 有同名方法，根据 Ruby 的继承规则，它们也可以调用这里的方法。

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

其它手法

```
view_context. (rails 3)
ActionController::Base.helpers.your_helper_methord or ApplicationController.helpers.your_helper_methord or YourController.helpers.your_helper_method
include helper in a singleton class and then singleton.helper
include the helper in the controller (WARNING: will make all helper methods into controller actions)
```

参考 [Rails - How to use a Helper Inside a Controller](http://stackoverflow.com/a/11161692/2174675)

