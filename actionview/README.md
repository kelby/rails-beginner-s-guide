# Action View

大致分为 3 块：渲染相关(renderer & rendering)，辅助方法(Helper)，非渲染和辅助方法

---

## 渲染相关(renderer & rendering)

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

### 模板

Type
类型
(也就是 format)

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
路径集
(供 ViewPaths 使用)

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

#### 处理器*

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


## 辅助方法(Helper)

ActionView 里的方法，尽管绝大部分都是 Helper 的形式对外提供，但仍然有那么少数几个不是。

|   内容     |   元素   | 表单       | record |
| :--------: |:--------:| :--------: | :----: |
| 生成内容   | 单元素   | 表单元素   | 弱关联 |
| 影响内容   | 复合元素 | 通用元素   | 强关联 |

### 生成内容

生成 HTML 内容的为生成内容。

### 影响内容

逻辑判断 + 生成内容或纯粹是逻辑，所有不能直观的看出生成什么 HTML 的元素

### 单元素

单独的 HTML 元素

### 复合元素

生成的内容是 HTML 元素，再包含 HTML 元素

因为表单实在是太重要了，所以区分一下是否表单元素

### 表单元素

四大金刚

FormBuilder 对 model 依赖最重，FormHelper 和 FormOptionsHelper 次之，FormTagHelper 依赖最轻。

FormBuilder 和 FormHelper 在方法、函数上基本是对应的。

FormBuilder 是面向对象，FormHelper 是面向函数。

### 通用元素(非表单元素)

区别于表单元素，这里的 helper 不用束缚于表单，比较通用。

另，表单元素其实也可以脱离表单来使用。所以，上面有提到 FormXxx 模块名起得不太合适。

### 与 model对象 弱关联

和后台数据关联不大，仅用于生成 HTML 元素。

以 "_tag" 结尾的 helper 都属于这一类，如 AssetTagHelper、FormTagHelper.

除此之外，还有其它，在此不一一列举。

### 与 model对象 强关联

和后台数据关系密切，需要有对象和/或其关联对象。

## 非渲染和辅助方法

不属于渲染相关、不属于 Helper 的模块和方法。

Base

RoutingUrlFor

RecordIdentifier

Railtie

ModelNaming

LogSubscriber

Layouts

Digestor
对视图进行加密，仅是片段缓存的组成部分。

~~DependencyTracker~~
供 Digestor 使用，digest 的时候可以以数组的形式传 dependencies 作为其可选参数。
