## ~~Translation~~

`human_attribute_name(attribute, options = {})`

根据属性名，自动转换成对人类阅读更加友好的字符串。如 "first_name" 转换成 "First name".

使用场景非常非常有限。举例：View 根据字段，自动生成的内容；格式良好的 Errors 报错信息等。

`lookup_ancestors()` 找出 respond_to?(:model_name) => true 的祖先。

> Note: 这里的方法用得很少，并且第一个方法里的"属性名"，其实不是属性也可以。
