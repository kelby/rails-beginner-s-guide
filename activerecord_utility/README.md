# Active Record 数据库特性及功能模块

**和数据库特性有关**，如：事务、锁。

Transactions 事务

Locking，包括 Optimistic、Pessimistic

**为了完成某项任务而生的功能模块，和 Active Model 类似**。包括但不限于：



* Validations 校验

包括 Associated Validator、Presence Validator、Uniqueness Validator.

Store

No Touching

Readonly Attributes

Nested Attributes 嵌套属性

Integration

Inheritance 单表继承

Enum 枚举

Callbacks 回调

Attributes

~~Translation~~

有多个模块是从 Base 抽取而来，它们是：Attribute Assignment、Dynamic Matchers、Inheritance、Integration、Model Schema、Querying、Readonly Attributes、Sanitization、Scoping、Translation

