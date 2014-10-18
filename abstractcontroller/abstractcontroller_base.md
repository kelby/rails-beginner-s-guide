## Base

ActionDispatch -> Metal -> AbstractController -> ActionController 请求是如何转变的？部分答案在这 ...

有方法：

```
abstract!
clear_action_methods!

controller_path
hidden_actions
internal_methods
method_added

action_methods
process

available_action?
supports_path?
```

下面只讲解一些可能有用的方法。

`controller_path()` 返回当前 Controller 所在的路径(包括目录、文件名)。例如，MyApp::MyPostsController 返回"my_app/my_posts"。

`action_methods()` 返回当前 Class 所包含的 action，默认等同于 public_instance_methods. 这里的 Class 可以是 Controller，也可以是 Mailer. 对于 AbstractController 来说，它们都是 Base 的子类，概念一样。

那么，如何获取当前程序所有的 Controller 和 action ?

获取所有的 Controller:

```ruby
ApplicationController.descendants
```

开发环境默认是延迟加载的，所以获取的只是当前 Controller，为了达到目的，需要先运行以下命令：

```ruby
Rails.application.eager_load!
```

生产环境，不必运行。

已经获取了所有的 Controller，那么获取某个 Controller 所有的 action，用刚才提到的 `action_methods` 即可：

```ruby
PostsController.action_methods
```
