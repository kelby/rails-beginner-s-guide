## Renderer

Rails 5 新增的类，让渲染模板不再依赖于 Controller.

原来的那一套渲染流程你是需要依赖于某个 Controller 这大环境的，但有的场景并不需要。

所以，现在拆分出来了。你可以不用依赖于 Controller，这样你能很方便的在 Job、Script 及 web sockets 里调用/渲染模板。

```ruby
ApplicationController.renderer.render template: '...'
```