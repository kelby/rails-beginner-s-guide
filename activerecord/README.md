# ActiveRecord

## 1 数据库操作(增删查改问)

## 2 工具、影响数据库操作

## 3 关联

- Associations

包括 builder (又包括：Association、BelongsTo、CollectionAssociation、HasAndBelongsToMany、HasMany、HasOne、SingularAssociation)

还包括 CollectionProxy

- AutosaveAssociation

~~AssociationRelation~~

- Aggregations

- Reflection

## 4 迁移

## 约定、配置

Timestamp

ModelSchema

- Core

从原来的 Base 中抽取出来单独成 module，负责部分底层对接、重写部分常用方法等。一起抽取出来的还有 Model 模块(包含 include、extend 等，现已经被删除)

~~AttributeSet~~

~~AttributeDecorators~~

- ~~Attribute~~

和属性的类型转换有关，在 AttributeSet 里会用到。

## 底层

知道有，但平时感受不到。

- Connection Handling

用来建立和数据库的连接。(配置、建立连接、日志，其中的建立连接，连接适配不是它做的！)

- Explain

explain 方法的底层实现(尽管是调用数据库的 explain...)

ExplainRegistry

ExplainSubscriber

QueryCache

Result

RuntimeRegistry

Sanitization

- Serialization

包括 XmlSerializer

- StatementCache

## 其它

- type

包括所有支持的数据类型

- Railtie

- LogSubscriber

- errors

- Base

- FixtureSet

- tasks

包括 DatabaseTasks，MySQLDatabaseTasks、PostgreSQLDatabaseTasks、SQLiteDatabaseTasks

- databases.rake (很重要)
