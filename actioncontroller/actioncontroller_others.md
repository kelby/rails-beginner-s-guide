# ActionController Others

`Mime::Type.register`

注册类型。每个请求都会显示或隐式的表明它所希望的响应格式，在这里注册后更容易找到对应的渲染器来处理。

`ActionController::Renderers.add`

上面的 `register` 中是登记，本身没有处理能力，这里添加渲染器进行处理。

## HideActions

Adds the ability to prevent public methods on a controller to be called as actions.

提供方法：

```ruby
hide_action
```