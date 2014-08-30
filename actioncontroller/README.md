# ActionController

## Metal 是重点

属于 middleware

包括但不限于 headers、response、request.
(另 env、session、params)

可直接继承使用

middleware_stack (这个很重要，最终指向 ActionDispatch::MiddlewareStack)

self.action + dispatch

## Metal 之外

有一些模块和 Metal 一样也有 `process_action` 方法，并且它们也被 include 进了 ActionController，并且我们自己定义的类没有这个方法(没有重写). 根据 Ruby 的代码执行规则，执行 process_action 时它们都会被执行(并且这个方法执行顺序先于 Metal 同名的方法)。

### cache


### 继承 AbstractController 的遗产

直接继承而来

### 使用 ActionDispatch 的资源

主要是 ActionDispatch 的 http 和 middleware 下的内容。

包括但不限于：

处理 request、response 相关和 headers 相关。(Metal 也可以做，但这里得到了发扬)

### 协作 ActionView

有很多类似的方法或丝丝关联。

### 少量的 ActiveModel

为了约定优于配置，很少的一部分方法和 ActiveModel 的 Naming 模块有关联

包括但不限于：

```
wrap_parameters

polymorphic_url
polymorphic_path
```

---

Controller Environment
The controller sets up the environment for actions (and, by extension, for the views that they invoke). Many of these methods provide direct access to infor- mation contained in the URL or request.

```
action_name: the name of the action currently being processed.
cookies: the cookies associated with the request, and setting values into this object stores cookies on the browser when the response is sent.
headers: a hash of HTTP headers that will be used in the response.
params: a hash-like object containing request parameters (along with pseudopa- rameters generated during routing).
request: the incoming request object.
response: the response object, filled in during the handling of the request, normally managed for you by Rails.
session: a hash-like object representing the current session data.
logger
```