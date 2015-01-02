# Action View 提供的辅助方法

下面 helper 分类，只是为了方便理清它们的结构。实际过程中，可交叉使用，能达到目的即可，有时候直接写 HTML 也是允许的。

有的方法根据其参数，可归于多个分类。

因为 Rails 背后会把所有 helper 方法(函数)都会被放进同一个 module 里，所以它们之间互相调用。

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
