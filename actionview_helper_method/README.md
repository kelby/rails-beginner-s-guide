# Action View 提供的辅助方法

## 分类

### 生成内容

生成 HTML 内容的为生成内容。

### 影响内容

逻辑判断 + 生成内容或纯粹是逻辑，所有不能直观的看出生成什么 HTML 的元素。

### 单元素

单独的 HTML 元素。

### 复合元素

生成的内容是 HTML 元素，再包含 HTML 元素。

因为表单实在是太重要了，所以区分一下是否表单元素。

举例：

`link_to` 是生成内容，且是单元素。

`button_to` 是生成内容，且是复合元素。

`link_to_unless_current` 和 `current_page?` 都属于影响内容。

### 表单元素

四大金刚：

Form Builder 对 model 依赖最重，Form Helper 和 Form Options Helper 次之，FormTagHelper 依赖最轻。

Form Builder 和 Form Helper 在方法、函数上基本是对应的。

Form Builder 是面向对象，Form Helper 是面向函数。

### 通用元素(非表单元素)

区别于表单元素，这里的 helper 不用束缚于表单，比较通用。

另，表单元素其实也可以脱离表单来使用。所以，上面有提到 FormXxx 模块名起得不太合适。

**包含但不限于以下 Helper**

Url Helper

Translation Helper

Text Helper

Tag Helper

Sanitize Helper

Rendering Helper

Record Tag Helper

Output Safety Helper

Number Helper

JavaScript Helper

Debug Helper

Date Helper

DateTime Selector

Csrf Helper

Controller Helper

Capture Helper

Cache Helper

Atom Feed Helper

Asset Url Helper

Asset Tag Helper

### 与 record 对象弱关联

和后台数据关联不大，仅用于生成 HTML 元素。

以 "_tag" 结尾的 helper 都属于这一类，如 AssetTagHelper、FormTagHelper.

除此之外，还有其它，在此不一一列举。

### 与 record 对象强关联

和后台数据关系密切，需要有对象和/或其关联对象。

最典型的如 form_for.

## 分类说明

上面 helper 分类，只是为了方便理清它们的结构。实际过程中，可交叉使用，能达到目的即可，有时候直接写 HTML 也是允许的。

有的方法根据其参数，可归于多个分类。

因为 Rails 背后会把所有 helper 方法(函数)都会被放进同一个 module 里，所以它们之间互相调用。

## 方法太多，看不过来？

有的 Rails 内置方法，难懂、难用，所以可以不用。以 ActionView 举例说明：

1. 我们需要看 API，才知道怎么传递参数，有什么参数；
2. 直接写 HTML，大脑不必转一圈；
3. 前端工程师不懂 Ruby，就看不懂；
4. 维护的时候也遇到上述 3 点困境。
