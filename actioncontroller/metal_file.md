### Metal 文件下的内容

**类方法**

```
action

use
middleware & middleware_stack

controller_name
```

**实例方法**

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

除此之外，还有：

```ruby
class_attribute :middleware_stack
self.middleware_stack = ActionController::MiddlewareStack.new
```
