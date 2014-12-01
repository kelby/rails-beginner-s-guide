## 渲染相关(Renderer & Rendering)

### 渲染器

TemplateRenderer  
模板渲染器

StreamingTemplateRenderer  
流模板渲染器

Renderer  
XML 渲染器

PartialIteration  
局部模板迭代

PartialRenderer  
局部模板渲染器

AbstractRenderer  
抽象出来的渲染器

### 模板(Template)

Type  
类型(也就是 format)

Text & HTML  
(什么也不是)

Resolver  
解析器

PathResolver  
路径解析器

FileSystemResolver  
文件系统解析器

OptimizedFileSystemResolver  
优化的文件系统解析器

FallbackFileSystemResolver  
回溯的文件系统解析器

Error  
各种错误

### 上下文

ViewPaths  
视图文件所在目录

Template  
视图文件

Rendering  
渲染(动词)

~~PathSet~~  
路径集(供 ViewPaths 使用)

LookupContext  
查找上下文

DetailsKey  
类似 cache key

DetailsCache  
为 Details 做缓存

~~OutputFlow & StreamingFlow~~  
输出流

~~Context~~  
上下文

~~OutputBuffer & StreamingBuffer~~  
输出流

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

