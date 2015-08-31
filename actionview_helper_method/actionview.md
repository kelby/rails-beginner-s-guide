## 与表单直接相关的辅助方法

按照代码结构上，可分为四类：

- Form Builder
- Form Helper
- Form Options Helper
- Form Tag Helper

按照使用方式不同，又可分为三类：

- Form Builder 对 Model 依赖最重，表单对象几乎等价于 record 对象。
- Form Helper 和 Form Options Helper 次之。
- Form Tag Helper 对 Model 依赖最轻，没有表单对象的概念，操作上几乎等价于 HTML（其实就是语法糖）。

Form Builder 和 Form Helper 在方法、函数上基本是对应的。

Form Builder 是面向对象，Form Helper 是面向函数。
