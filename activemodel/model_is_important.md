## Model - 核心

这里指的是 ActiveModel::Model，它及它下面所包含的 Validations、Conversion 和 Naming、Translation 等模块是整个 Active Model 里的核心部分。

Model 做的事情包括但不限于: **校验** 和 **抽象ORM接口**。

Conversion & Validations 是 `include` 如果没有指明 ClassMethods，则引入的是实例方法；
<br>
Naming & Translation 是 `extend` 默认引入的就已经是类方法。
注意这点小差别，对于使用上会有帮助。
