## 渲染相关(Renderer & Rendering)

渲染是它的老本行。

每次都要指定模板文件，并且说一遍渲染，不麻烦吗？<br>
因为模板文件，都在同一目录下，所以我们不必这么麻烦。引进 ActionView 的子模块 Rendering 后我们设置默认的目录即可，并且能自动帮我们"说一遍渲染"。

分为 3 大块：渲染器、模板和上下文。

### 1) 渲染器

Template Renderer  
模板渲染器

Streaming Template Renderer  
流模板渲染器  
为了支持"streaming 流"！

Renderer  
XML 渲染器

Partial Iteration  
局部模板迭代

Partial Renderer  
局部模板渲染器

Abstract Renderer  
抽象出来的渲染器

### 2) 模板内容(Template)

Type  
类型(也就是 format)

Text & HTML  
(什么也不是)

Resolver  
解析器

Path Resolver  
路径解析器

File System Resolver  
文件系统解析器

Optimized File System Resolver  
优化的文件系统解析器

Fallback File System Resolver  
回溯的文件系统解析器

Error  
各种错误

### 3) 上下文(view_context)

View Paths  
视图文件所在目录

Template  
视图文件

Rendering  
渲染(动词)

~~PathSet~~  
路径集(供 View Paths 使用)

Lookup Context  
查找上下文

DetailsKey  
类似 cache key

DetailsCache  
为 Details 做缓存

~~Context~~  
上下文  
为了 Action Controller 可以创建 ActionView::Base 的实例对象，然后进行渲染模板的工作。  
(从 Base 里抽取出来的，提供了 @output_buffer, @view_flow 和 @virtual_path)

~~Output Flow & Streaming Flow~~  
输出流  
为了支持"streaming 流"！

~~Output Buffer & Streaming Buffer~~  
输出缓冲区  
为了支持"streaming 流"！

**处理器***

Handlers  
处理器

Raw  
原生的处理器

Erubis  
Erubis 处理器

ERB  
ERB 处理器

Builder  
XML 处理器

### 4) 查找上下文(lookup_context)

TODO
