## Metal 介绍

metal 里的 middleware_stack 循环执行，metal 之外的东西是附属品。

一般模块名和同名目录都是有联系的，但 metal 不是，单指的是 metal.rb 这个文件，它和 metal/ 目录下的文件及内容没有关系。

---

MVC 里的 C 可以做得很精简，ActionController::Metal 就是例子。除了提供一个有效的 Rack 接口外，它几乎没有任何其它功能。

举个例子:

```ruby
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

在路由里添加相应代码，将请求转发到刚才的 HelloController#index 进行处理:

```ruby
# in your config/routes.rb
get 'hello', to: HelloController.action(:index)
```

为了让 Route 能够很好转发，action 方法会返回一个有效的 Rack application.

## Others

不反对给 Rails 进行瘦身，但不要盲目，要清楚自己在做什么。

另外，要清楚的知道各个组件有什么用，添加是为了什么，去掉又会有什么影响。

---

为什么能够连续调用，原因：

你看每个 Rack Middleware 的 call 函数的最后一行，是不是都是 @app.call(env)
这说明，它在调用下一个 middleware 啊

它们是一条封闭的链接，一直走下去，最后又会回到开头处，并且中间只要有一处断了，那整条链子就都 走不通！

顺序是：默认是按 use 的顺序走下去，但 use 时你也是可以指定的。

> Note: @app 和 env 一直在变，但又一直没变。

## Metal 提供的方法：

```
# Class Public methods
action(name, klass = ActionDispatch::Request)
call(env)
controller_name()
middleware()
new()
use(*args, &block)

# Instance Public methods
_status_code()
content_type()
content_type=(type)
controller_name()
env()
location()
location=(url)
params()
params=(val)
performed?()
response_body=(body)
status()
status=(status)
url_for(string)
```
