## 其它

#### Metal 增强模块和 Middleware 的区别

Middleware 是在请求进入 Controller#action 之前，而 Metal 增强组件是在请求进入 Controller#action 之后。


Middleware 需要的 `app` 和 `@env`，而 Metal 增强组件需要的是 Controller 或 action.
