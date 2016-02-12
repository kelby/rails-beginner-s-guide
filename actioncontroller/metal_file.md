## Metal 文件下的内容

Metal 是请求从路由到 Controller 的中转站。

**类方法：**

```
action

use
middleware & middleware_stack

controller_name
```

**实例方法：**

```
dispatch

content_type
content_type=

location
location=

params
params=

status
status=

performed?

response_body=

controller_name

env

url_for
```

`self.action` 和 `dispatch` 为转移到下一站场起到了很大的作用。

```ruby
# Action Dispatch 转发过来的请求，要先经过层层的 middleware 处理，才能到达指定的 action.
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

从堆栈里取 middleware 并处理。

`use` 方法把 middleware 放入 middleware stack，也很重要。

```ruby
class PostsController < ApplicationController
  use AuthenticationMiddleware, except: [:index, :show]
end
```

除此之外，需要清楚：

```ruby
class_attribute :middleware_stack
self.middleware_stack = ActionController::MiddlewareStack.new
```
