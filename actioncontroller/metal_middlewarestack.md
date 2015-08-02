## Middleware Stack

继承于 Action Dispatch 的 MiddlewareStack，用于存放 middleware.
<br>
它的功能和父类一样，只是作用的层面不同。

Middleware 并不总是需要项目级别的，它也可以精确到某个 Controller，甚至是 action.

```ruby
class PostsController < ApplicationController
  use AuthenticationMiddleware, except: [:index, :show]
end
```

`self.use` 是前面 Metal 提供的方法，可以添加 middleware 到堆栈，它们在最后执行。<br>
(这里的堆栈指的可以是 ActionController::MiddlewareStack，也可以是 ActionDispatch::MiddlewareStack).
