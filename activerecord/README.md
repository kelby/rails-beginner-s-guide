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

配置 database.yml，及一些看不出来什么功能的东西

~~AttributeSet~~

~~AttributeDecorators~~

~~Attribute~~

## 底层

知道有，但平时感受不到。

ConnectionHandling

Explain

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
