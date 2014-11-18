## Model 是重点

从普通Web开发者角度来看，ActiveModel 最重要的就是 Model。它又可分为: **校验** 和 **抽象ORM接口**，主要由 Naming，Conversion，Translation 和 Validations 等子模块组成。

Conversion & Validations 是 `include` 如果没有指明 ClassMethods，则引入的是实例方法；  
Naming & Translation 是 `extend` 默认引入的就已经是类方法。
注意这点小差别，对于使用上会有帮助。
