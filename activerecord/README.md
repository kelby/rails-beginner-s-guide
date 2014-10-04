# ActiveRecord

## 迁移

Migration

---

SchemaMigration

SchemaDumper

Schema

---

下面这 3 个模块，来源 ConnectionAdapters，它下面还有很多模块，在此忽略其它 ...

SchemaStatements

TableDefinition

Table

---

DatabaseTasks

## 数据库操作(增删查改问)

- locking

包括 Optimistic、Pessimistic

- Scoping

包括 Default、Named

- Relation

包括 SpawnMethods、QueryMethods、~~PredicateBuilder~~、~~Merger~~、FinderMethods、~~Delegation~~、Calculations、Batches

- Querying

Persistence

NullRelation

- ~~DynamicMatchers~~

将被废除 gem 'activerecord-deprecated_finders'

- CounterCache

AttributeAssignment

- AttributeMethods

包括 BeforeTypeCast、Dirty、PrimaryKey、Query、Read、Serialization、TimeZoneConversion、Write

## 关联

- Associations

包括 builder (又包括：Association、BelongsTo、CollectionAssociation、HasAndBelongsToMany、HasMany、HasOne、SingularAssociation)

还包括 CollectionProxy

- AutosaveAssociation

~~AssociationRelation~~

- Aggregations

- Reflection

## 约定、配置

Timestamp



ModelSchema

- Core

配置 database.yml，及一些看不出来什么功能的东西



~~AttributeSet~~



~~AttributeDecorators~~

~~Attribute~~

## 工具、影响数据库操作

为了完成某项任务而生。

Transactions

- Validations

包括 AssociatedValidator、PresenceValidator、UniquenessValidator

Translation

Store

NoTouching

ReadonlyAttributes



NestedAttributes

Integration

- Inheritance

model 之间的继承关系(用的是同一张表)



- Enum

Callbacks

~~Attributes~~

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

对于 Association 你还有什么要说的？  
http://jonathanhui.com/ruby-rails-3-model-association

对 Association 里的 has_many 多讲解一点？  
http://depthcoding.ybarthelemy.info/2012/07/30/under-the-hoods-of-activerecords-has_many-method/

