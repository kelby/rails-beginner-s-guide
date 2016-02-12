## Reflection - 核心之关联两者

#### 关系图

```
HasManyReflection & HasOneReflection & BelongsToReflection & HasAndBelongsToManyReflection RuntimeReflection
                          |                                                                |
                          V                                                                V
AggregateReflection & AssociationReflection                              PolymorphicReflection
                    |                                                       |
                    V                                                       V
          MacroReflection &                                     ThroughReflection
                          |
                          V
                AbstractReflection
```

**延续 build 目录下 association 文件的工作。**

一个很重要的概念，包含了所有的关联信息。
包括但不限于：用的是什么关联、关联对象名字、可选参数等。

一般关联和 aggregate 要区分开来。前者用 _reflections，后者用 aggregate_reflections.

提供方法：

#### 1) 模块方法

```
create

add_reflection
add_aggregate_reflection
```

`create`
<br>可以创建 Aggregate Reflection，Has Many Reflection、Has One Reflection 和 Belongs To Reflection 4 种关联；
<br>如果使用了 :through 则还会自动生成 Through Reflection 关联。

#### 2) 类方法

```ruby
reflections # 所有正常的关联
reflect_on_all_associations          # 指定 macro 的 reflections
reflect_on_all_autosave_associations # 包含 autosave: true 的 reflections

reflect_on_all_aggregations # 所有 aggregate 关联

# 以下两方法要提供被关联对象名字
reflect_on_association
reflect_on_aggregation
```









#### ~~10) Has And Belongs To Many Reflection~~

继承于 Association Reflection

```
macro

collection?
```

#### ~~11) Through Reflection~~

继承于 Abstract Reflection

```ruby
attr_reader :delegate_reflection

delegate :foreign_key, :foreign_type, :association_foreign_key,
         :active_record_primary_key, :type, :to => :source_reflection

klass
source_reflection
through_reflection
chain
scope_chain
join_keys

nested?
association_primary_key
source_reflection_names
source_reflection_name
source_options
through_options
join_id_for
check_validity!
```

#### 使用举例

```ruby
User.reflections.keys
=> [:comments,
 :warehouse]

User.reflections.each_pair { |a, x| puts [a, x.macro].join(' => ') };
=> comments  => has_many
   warehouse => belongs_to

User.reflections.values.first.class
=> ActiveRecord::Reflection::AssociationReflection

r = User.reflections[:warehouse]
=> #<ActiveRecord::Reflection::AssociationReflection:0x007ff4606c66d0
 @active_record=
  User(id: integer, login: string, email: string, warehouse_id: integer),
 @class_name="Warehouse",
 @collection=false,
 @klass=
  Warehouse(id: integer, name: string),
 @macro=:belongs_to,
 @name=:warehouse,
 @options={},
 @plural_name="warehouses",
 @quoted_table_name="`warehouses`",
 @table_name="warehouses">

r.active_record
=> User(id: integer, login: string, email: string, warehouse_id: integer)

Post.reflections[:comments].table_name       # => "comments"
Post.reflections[:comments].macro            # => :has_many
Post.reflections[:comments].primary_key_name # => "message_id"
Post.reflections[:comments].foreign_key      # => "message_id"
```

#### 其它

Reflection 虽然很重要，但对于普通 Web 开发者而言，使用场景有限，一般不会直接使用。下面是我想到的一些使用场景，供参考：

1. 动态创建其关联对象的实例，如：在表单里点击按钮，创建一个嵌套对象(属性)。
2. 查看使用 gem 后引进了什么关联。
3. 删除某个重要对象时，删除所有与之关联的对象。预防用 :dependent 或手动删除会有遗漏。
