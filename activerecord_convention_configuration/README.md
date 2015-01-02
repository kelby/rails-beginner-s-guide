# Active Record 约定、配置

- Timestamp

- Model Schema

- Core

从原来的 Base 中抽取出来单独成 module，负责部分底层对接、重写部分常用方法等。一起抽取出来的还有 Model 模块(包含 include、extend 等，现已经被删除)

- ~~Attribute Set~~

- ~~Attribute Decorators~~

- ~~Attribute~~

和属性的类型转换有关，在 Attribute Set 里会用到。
