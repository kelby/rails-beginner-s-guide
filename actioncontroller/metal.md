## Metal - 加强的 Rack, 简陋的 Controller

**加强的 Rack:**

意味着它符合 Rack 接口规范，可以直接使用它，创建出来的应用可以看做是一个 Rack application.

在 Rack 的基础上，它增加了 middleware_stack 的预处理。

和 Rack 一样，它的功能真的很有限。如你的项目做为 API 对外提供服务，不需要那么多功能，你可以尝试。

和 Rack 一样，相对来说它的性能比较高。如你的 Rails 项目对性能要求非常高，你可以尝试。

> Rails Metal is a subset of Rack middleware 可以参考【Rack - Ruby Web server 接口】章节了解更多关于 Rack 的内容。

**简陋的 Controller:**

除了提供一个有效的 Rack 接口外，它几乎没有任何其它功能。

ActionController::Base 在它基础之上添加了多个类和模块，这使得功能得到增多，同时在性能上也会有相应损耗。如果你觉得这些功能不是必需的，或者性能的损耗是不可忍受的，你可以直接使用 Metal.

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
# config/routes.rb
get 'hello', to: HelloController.action(:index)
```

为了让 Route 能够很好转发，action 方法会返回一个有效的 Rack application.

#### 主要做的事情

调用 middleware 进行预处理。

像 Rack application 一样小而完整的处理。

做出响应。

#### 其它

一般模块名和同名目录都是有联系的，但 metal 不是，它单指的是 metal.rb 这个文件，和 metal/ 目录下的文件及内容没有关系。

- 直接使用 Metal 时，要清楚自己在做什么。

另外，要清楚的知道各个组件有什么用，添加是为了什么，去掉又会有什么影响。

- 为什么能够连续调用，原因：

你看每个 Rack Middleware 的 `call` 函数的最后一行，是不是都是 `@app.call(env)` ？
这说明，它在调用下一个 middleware.

它们是一条封闭的链接，一直走下去，最后又会回到开头处，并且中间只要有一处断了，那整条链子就都走不通。

顺序是：默认是按 `use` 的顺序走下去，但 use 时你也是可以指定的。

> Note: @app 和 env 内容一直在变，但本质又一直没变。

不再推荐使用字符串引入 middleware 所以

```
middleware.use "Foo::Bar"
```

变为

```
middleware.use Foo::Bar
```
