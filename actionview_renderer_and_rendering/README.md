# Action View 渲染相关

包括"渲染器(n)"和"渲染(v)"，即：Renderer & Rendering.

每次都要指定模板文件，并且说一遍渲染，不麻烦吗？<br>
因为模板文件，都在同一目录下，所以我们不必这么麻烦。引进 Action View 的子模块 Rendering 后我们设置默认的目录即可，并且能自动帮我们"说一遍渲染"。

分为 4 大块：渲染器、模板、上下文和查找模板对象。(以下只是粗略的分类)

#### 1) 渲染器

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

#### 2) 模板(Template)

对应文件及同名目录。

#### 3) 上下文(view_context)

Rendering  
渲染(动词)

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

#### 4) 查找模板对象(lookup_context)

View Paths  
视图文件所在目录

~~PathSet~~  
路径集(供 View Paths 使用)

DetailsKey  
类似 cache key

DetailsCache  
为 Details 做缓存

Lookup Context  
查找上下文
