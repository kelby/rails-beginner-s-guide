## Base

Action Dispatch -> Metal -> Abstract Controller -> Action Controller 请求是如何转变的？部分答案在这 ...

此外，还有一些平时用得不多，但比较有趣的方法。

重要的实例方法有：

```
process
```

重要的私有方法有：

```
process_action

send_action & send
```

重要的类方法有：

```
abstract!

action_methods
```

`action_methods` 返回当前类所包含的 action，默认等同于 public_instance_methods. 这里的类可以是 Controller，也可以是 Mailer. 对于 Abstract Controller 来说，它们都是 AbstractController::Base 的子类，概念一样。

Action Dispatch 里转发先到 ActionController::Metal::action, 然后到 ActionController::Metal#dispatch, 接着到 AbstractController::Base#process 也就是这里的 `process` 方法，然后到 `process_action` 和 `send_action & send`，最后到具体的 action 进行处理。

其它实例方法：

```
controller_path

action_methods

available_action?
```

其它类方法：

```
clear_action_methods!

controller_path

hidden_actions

internal_methods

method_added(name)

supports_path?
```

`controller_path` 返回当前 Controller 所在的路径(包括目录、文件名)。例如，YourApp::PostsController 返回"your_app/posts".

其它私有方法：

```
action_method?

method_for_action
```
