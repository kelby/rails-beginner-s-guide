## Base

请求从 Route 到 Controller#actions 是如何转变的？魔法(答案)在这，从 process 到 _find_action_name、process_action ...

还有一些平时用得不多，但比较有趣的方法。

Action Dispatch -> Metal -> Abstract Controller -> Action Controller 请求是如何转变的？部分答案在这 ...

实例方法重要的有：

```
process

action_methods

# 以下几个为私有方法
process_action

send_action & send
```

`process` routes.rb 里转发先到 ActionController::Metal::action 然后到 ActionController::Metal#dispatch 接着到 AbstractController::Base#process 也就是这里的 process 方法，然后进行后续处理。
<br>
`process` 会调用到下面的 process_action 和 send_action & send，以便让具体 action 处理请求。

`action_methods()` 返回当前 Class 所包含的 action，默认等同于 public_instance_methods. 这里的 Class 可以是 Controller，也可以是 Mailer. 对于 Abstract Controller 来说，它们都是 Base 的子类，概念一样。

其它实例方法：

```
available_action?

controller_path
```

类方法重要的有：

```
abstract!

action_methods
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

`controller_path()` 返回当前 Controller 所在的路径(包括目录、文件名)。例如，YourApp::PostsController 返回"your_app/posts".
