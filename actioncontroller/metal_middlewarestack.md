## Middleware Stack

继承于 ActionDispatch::MiddlewareStack (有 Middleware)
<br>
对应的，也有 Middleware

middleware 并不总是需要项目级别的，它也可以精确到某个 Controller，甚至是 action.

```ruby
class PostsController < ApplicationController
  use AuthenticationMiddleware, except: [:index, :show]
end
```

`self.use` 是前面 Metal 提供的方法，可以添加 middleware 到堆栈，它们在最后执行。<br>
(这里的堆栈指的是 ActionController::MiddlewareStack，也是 ActionDispatch::MiddlewareStack).
