
AbstractController 和 ActionMailer 有关联。ActionMailer::Base 继承于 AbstractController::Base

AbstractController 和 ActionController 有关联。ActionController::Base 继承于 ActionController::Metal，然后 ActionController::Metal 又继承于 AbstractController::Base

AbstractController 还和 ActionDispatch 以及 ActionView 有关联，会直接或间接调用它们的方法。

非常典型的例子：

**render**

AbstactController 封装 ActionView
然后，给 ActionController 和 ActiveMailer 使用
而 ActionController 根据自己情况又有自己的 render

**url_for**

AbstactController 封装了 ActionDispatch，然后给 ActionController、ActiveMailer 使用
而 ActionController 根据自己情况又有自己的 url_for

Collector 是 AbstactController 实现的。
ActionController 有自己的扩展
ActiveMailer 也有自己的扩展

其它几个：asset_paths, callbacks, helpers, translation 并没有被扩展，都是 AbstractController 原装。
