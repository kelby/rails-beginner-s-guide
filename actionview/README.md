# ActionView

大致分为 3 块：

---

## 渲染相关(renderer & rendering)

ViewPaths
视图文件所在目录

Template
视图文件

Rendering
渲染(动词)

PathSet
路径集
(供 ViewPaths 使用)

LookupContext
查找上下文

DetailsKey
类似 cache key

DetailsCache
为 Details 做缓存

OutputFlow & StreamingFlow
输出流

Context
上下文

OutputBuffer & StreamingBuffer
输出流

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

#### 处理器

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





## 辅助方法(Helper)

ActionView 里的方法，尽管绝大部分都是 Helper 的形式对外提供，但仍然有那么少数几个不是。

|   内容     |   元素   | 表单       |
| :--------: |:--------:| :--------: |
| 生成内容   | 单元素   | 表单元素   |
| 影响内容   | 复合元素 | 通用元素   |

### 生成内容

生成 HTML 内容的为生成内容。

### 影响内容

逻辑判断 + 生成内容或纯粹是逻辑，所有不能直观的看出生成什么 HTML 的元素

### 单元素

单独的 HTML 元素

### 复合元素

生成的内容是 HTML 元素，再包含 HTML 元素

因为表单实在是太重要了，所以区分一下是否表单元素

举例：

`link_to` 是生成内容，且是单元素

`button_to` 是生成内容，且是复合元素

`link_to_unless_current` 和 `current_page?` 都属于影响内容。

### 表单元素

四大金刚

FormBuilder 对 model 依赖最重，FormHelper 和 FormOptionsHelper 次之，FormTagHelper 依赖最轻。

FormBuilder 和 FormHelper 在方法、函数上基本是对应的。

FormBuilder 是面向对象，FormHelper 是面向函数。

**FormBuilder**

A FormBuilder object is associated with a particular model object and allows you to generate fields associated with the model object.

你要修改的是 model 对象，途径是通过表单实现。
所以，model 对象和表单就必需有某种联系。
在这里，这种联系由 FormBuilder 及其实例对象完成。

The FormBuilder object can be thought of as serving as a proxy for the methods in the FormHelper module.

3 个来源：

```
date_helper.rb
form_helper.rb
form_options_helper.rb
```

大部分是封装 @template

大部分以 f.xxx 的形式调用

> **Note:** f 就是 FormBuilder 的实例对象，所以可以调用 FormBuilder 的实例方法。

构建器怎么来？

```ruby
# form_for 和 fields_for 调用 instantiate_builder

# 再调用
default_form_builder
  builder = ActionView::Base.default_form_builder
  ... ...
end

ActiveSupport.on_load(:action_view) do
  cattr_accessor(:default_form_builder) { ::ActionView::Helpers::FormBuilder }
end

# 简单理解 builder = FormBuilder

# 最后调用
builder.new(object_name, object, self, options)
```

**FormHelper**

FormHelper 是面向函数
函数(f对象，参数)

FormBuilder 是面向对象

f对象.方法(参数)

前者，更灵活。因为它没有限定对象，有时候我们'需要'这样做，而有时候我们没'必要'。

form_for 和 fields_for 是另类

大部分封装 Tags::Xxx

名字 FormHelper 起得有一点不合适。

**FormOptionsHelper**

Provides a number of methods for turning different kinds of containers into a set of option tags.

常见于两个对象之间

分为目的 和 手段。

比如：select_tag 是目的，options_for_select 是手段。

名字 FormOptionsHelper 起得有一点不合适。

**FormTagHelper**

Provides a number of methods for creating form tags that don't rely on an Active Record object assigned to the template like FormHelper does. Instead, you provide the names and values manually.

最接近 HTML 代码

没有对象(不用对应 model)及类似概念

封装 tag 或 content_tag 而来

名字 FormTagHelper 起得非常不合适

### 通用元素

区别于表单元素，这里的 helper 不用束缚于表单，比较通用。

另，表单元素其实也可以脱离表单来使用。所以，上面有提到 FormXxx 模块名起得不太合适。

### 包含但不限于以下 Helper

UrlHelper

TranslationHelper

TextHelper

TagHelper

SanitizeHelper

RenderingHelper

RecordTagHelper

OutputSafetyHelper

NumberHelper

JavaScriptHelper

DebugHelper

DateHelper

DateTimeSelector

CsrfHelper

ControllerHelper

CaptureHelper

CacheHelper

AtomFeedHelper

AssetUrlHelper

AssetTagHelper

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

DependencyTracker
