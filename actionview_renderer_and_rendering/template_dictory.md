## template 目录

包含了：error、handlers、html、resolver、text、types 等。<br>
其中 handlers 又包含了：builder、erb、raw 等。

其中，Html 和 Text 这两个类为模板(较简单)，handlers 下面的 Builder、ERB 和 Raw 3 个类也是模板(较复杂)。

从上述可知，Renderer 其实也没做什么，处理工作交给 Template 进行处理。

```
Renderer(最外层的接口)
      |
      V
各个 Renderer(中间处理)
      |
      V
Template(最底层的处理)
```

template/ 目录里的各个 handler，执行的是与 Rails 环境无关的解析、渲染工作。

------

Template  
视图文件

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

Handlers  
处理器，包括：

- Raw  
原生的处理器

- Erubis  
Erubis 处理器

- ERB  
ERB 处理器

- Builder  
XML 处理器
