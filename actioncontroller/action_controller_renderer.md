## Renderer

让渲染模板需要的环境变得更容易，不必强制依赖于具体的 Controller#action.

```ruby
ApplicationController.renderer.render template: '...'
# 更简短的写法
ApplicationController.render template: '...'

ApplicationController.renderer.new(method: 'post', https: true)
```

原来的那一套渲染流程你是需要依赖于某个 Controller 这大环境的，但有的场景并不需要（比如使用 Action Cable 时）。

所以，现在拆分出来了。你可以不用依赖于 Controller，这样你能很方便的在 Job、Script 及 web sockets 里调用/渲染模板。

`renderer` 获取渲染器实例：

```ruby
ApplicationController.renderer
```

它所依赖的环境 `@env` 像 Rack 一样，非常简单：

```ruby
ApplicationController.renderer.defaults
```