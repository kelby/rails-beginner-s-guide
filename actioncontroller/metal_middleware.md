## Base - Metal 的继承者

相对于 Metal，它包含了各个控制器上能够使用到的组件。所以使用它时，在性能上会比直接继承使用 Metal 慢得多，但我们可用的功能更丰富了。

我们看看 ActionController::Base 引入了哪些模块：

```ruby
MODULES = [
  AbstractController::Rendering,
  AbstractController::Translation,
  AbstractController::AssetPaths,

  Helpers,
  UrlFor,
  Redirecting,
  ActionView::Layouts,
  Rendering,
  Renderers::All,
  ConditionalGet,
  EtagWithTemplateDigest,
  RackDelegation,
  Caching,
  MimeResponds,
  ImplicitRender,
  StrongParameters,

  Cookies,
  Flash,
  RequestForgeryProtection,
  ForceSSL,
  Streaming,
  DataStreaming,
  HttpAuthentication::Basic::ControllerMethods,
  HttpAuthentication::Digest::ControllerMethods,
  HttpAuthentication::Token::ControllerMethods,

  AbstractController::Callbacks,

  Rescue,

  Instrumentation,

  ParamsWrapper
]

MODULES.each do |mod|
  include mod
end
```

引入了这么多模块，虽然方便了使用。但有的模块，我们用不到，浪费了。

如果对性能有很高要求，并且知道各个模块作用的话，可以适当去掉某些模块。

> 后文讲到的【API】是精简版的 Base，它也是直接继承于 Metal，在 API 模式里可用。
