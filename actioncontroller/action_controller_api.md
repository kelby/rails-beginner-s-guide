## API - Metal 的继承者

精简版的 ActionController::Base，为 API 而生。

它没有也不需要 layouts 和 templates rendering, cookies, sessions, flash, assets 等。

- 默认只有 ApplicationController 继承于它
- 没有默认渲染。所以需要明确指定渲染
- 除了渲染，也可以有重定向
- 可用 include 引入其它模块，比如 MimeResponds

它和 ActionController::Base 本质上是一样的，对比来说，它去掉了很多不必要的 Metal 增强组件，所以性能会比较快：

```ruby
MODULES = [
  AbstractController::Rendering,

  UrlFor,
  Redirecting,
  ApiRendering,
  Renderers::All,
  ConditionalGet,
  BasicImplicitRender,
  StrongParameters,

  ForceSSL,
  DataStreaming,

  AbstractController::Callbacks,

  Rescue,

  Instrumentation,

  ParamsWrapper
]

MODULES.each do |mod|
  include mod
end
```
