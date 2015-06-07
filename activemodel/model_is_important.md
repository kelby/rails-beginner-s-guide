## Model - 核心

这里指的是 ActiveModel::Model，它及它下面所包含的 Validations、Conversion 和 Naming、Translation 等模块属于 Active Model 里的核心部分。

从普通 Web 开发者角度来看，Active Model 最重要的就是 Model. 它又可分为: **校验** 和 **抽象ORM接口**，主要由 Naming，Conversion，Translation 和 Validations 等子模块组成。

Conversion & Validations 是 `include` 如果没有指明 ClassMethods，则引入的是实例方法；  
Naming & Translation 是 `extend` 默认引入的就已经是类方法。
注意这点小差别，对于使用上会有帮助。
