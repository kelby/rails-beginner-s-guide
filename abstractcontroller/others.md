## 和其它模块的关系及几个比较有意思的方法

Abstract Controller 和 Action Mailer 有关联。ActionMailer::Base 继承于 AbstractController::Base

Abstract Controller 和 Action Controller 有关联。ActionController::Base 继承于 ActionController::Metal，然后 ActionController::Metal 又继承于 AbstractController::Base

Abstract Controller 还和 Action Dispatch 以及 Action View 有关联，会直接或间接调用它们的方法。

非常典型的例子：

**render**

Abstact Controller 封装 Action View
然后，给 Action Controller 和 Active Mailer 使用
而 Action Controller 根据自己情况又有自己的 render.

**url_for**

Abstact Controller 封装了 Action Dispatch，然后给 Action Controller、Active Mailer 使用
而 Action Controller 根据自己情况又有自己的 url_for.

Collector 是 Abstact Controller 实现的。Action Controller 有自己的扩展，Active Mailer 也有自己的扩展。

其它几个：asset_paths, callbacks, helpers, translation 并没有被扩展，都是 Abstract Controller 原装。
