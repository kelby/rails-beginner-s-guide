Rails 是如何启动的？<br>
在 Rails 里启动命令 rails s，会启动当前应用。<br>
app 为 Rack::Builder 实例<br>
server 为 Rack::Handler 实例<br>
所以，我不能直接回答这个问题，因为启动 Rails，实际上就是启动一个 Rack。

既然Rails用于Web开发，总得有个入口吧。输入的 url 是怎么处理的？<br>
url 是怎么解析，太底层的东西我们就不说了。解析后到 Route，检查路由表，如果符合要求则"转发"到相应的 Controller#action

路由里显示，请求会被转发到对应的 Controller#action 处理，怎么"转发"的？<br>
由 Rack 的概念，我们可以引申出 middleware。实际上每一个 Controller#action 也是一个 middleware，所以你可以通过 call 方法，传递 env 调用它们。例如：controller.action(action).call(env)
嗯，转发就是这么做的。

Controller#action 调用，可是 action 并不是类方法啊，并且每个 action 都要传递 env ?<br>
ActionController 的子模块 Metal 里有 self.action(name, klass = ActionDispatch::Request) 方法，可以帮我们做到这一点。

每次都要指定模板文件，并且说一遍渲染，不麻烦吗？<br>
因为模板文件，都在同一目录下，所以我们不必这么麻烦。引进 ActionView 的子模块 Rendering 后我们设置默认的目录即可，并且能自动帮我们"说一遍渲染"。

