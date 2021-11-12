# Abstract Controller

Abstract Controller 做的事情很有限，**Controller 和 Mailer 的共同部分、基础模块**。

> 我们可以看到 Mailer 和 Controller 在形式上有很多相似的部分，可以简单理解这部分由 Abstract 完成。

无论是它自己定义的方法，还是封装 Action View 和 Action Dispatch 得到的方法，最终都提供给 Action Controller 和 Action Mailer 使用。

这些方法\(或模块\)包含但不限于渲染\(模板或局部模板\)、Helpers、Callbacks、Mime、Url For 等。

它服务于 Action Controller 和 Action Mailer，**找到真正的 action**.

* 辅助 ActionController::Base 将战场转移到具体的 Controller\#action\(经过 Metal\).
* 辅助 ActionMailer::Base 将战场转移到具体的 Mailer\#action.

准备及转移工作在之前已经完成了，它只是在最后一步，调用具体 action.

