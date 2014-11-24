## AbstractController

- 服务于 ActionController 和 ActionMailer
- 将站场从 ActionDispatch 转移到 Controller 或 Mailer

AbstractController 无论是它自己定义的方法，还是封装 ActionView 和 ActionDispatch 得到的方法，最终都提供给 ActionController 和 ActionMailer 使用。  
这些方法(或模块)包含但不限于渲染(模板或局部模板)、Helper相关、回调、Mime、UrlFor 等。

AbstractController 起到了承上启下的作用(所以，有时候它的代码会让你觉得很迷惑)，及 ActionDispatch 到 ActionController 转换，并且也实现了一些重要的方法，如：Callbacks、Helpers.

下面只挑选部分模块来讲解，其它的模块，可以参考"源码剖析"。

