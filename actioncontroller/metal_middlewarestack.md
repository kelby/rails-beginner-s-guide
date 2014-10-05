## Middleware Stack

middleware 并不总是需要项目级别的，它也可以精确到某个 Controller，甚至是 action.

```ruby
class PostsController < ApplicationController
  use AuthenticationMiddleware, except: [:index, :show]
end
```

`self.use` 可以添加 middleware 到堆栈，它们在最后执行。

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
