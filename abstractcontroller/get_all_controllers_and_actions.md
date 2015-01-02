### 获取所有的 Controller 和 action

如何获取当前程序所有的 Controller 和 action ?

获取所有的 Controller:

```ruby
ApplicationController.descendants
```

开发环境默认是延迟加载的，所以获取的只是当前 Controller，为了达到目的，可以先运行以下命令：

```ruby
Rails.application.eager_load!
```

生产环境，不必运行。

已经获取了所有的 Controller，那么获取某个 Controller 所有的 action，用 `action_methods` 方法即可：

```ruby
PostsController.action_methods
```
