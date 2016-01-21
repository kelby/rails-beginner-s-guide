#### ~~Context~~

部分上下文内容。

从 Base 里抽取出来的，提供了 @output_buffer, @view_flow 和 @virtual_path

为了 Action Controller 可以创建 ActionView::Base 的实例对象，然后进行渲染模板的工作。  

不要被名字欺骗了，它只是 `view_context` 很小的组成部分。
