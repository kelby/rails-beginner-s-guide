# ActiveRecord Association Reflection

## Reflection

一个很重要的概念，包含了所有的关联信息。
包括但不限于：用的是什么关联、关联对象名字、可选参数等。

### self

```
create
add_reflection
add_aggregate_reflection
```

### ClassMethods

```
reflect_on_aggregation, reflect_on_all_aggregations
reflect_on_all_associations, reflect_on_all_autosave_associations
reflect_on_association, reflections
```

### AssociationReflection

AssociationReflection 继承于 MacroReflection 又继承于 AbstractReflection

另，AggregateReflection 也继承于 MacroReflection

```ruby
:klass
:table_name
:quoted_table_name
:foreign_type
:type
:primary_key_column
:foreign_key
:association_primary_key
:association_foreign_key
:active_record_primary_key
:counter_cache_column
:columns
:through_reflection
:source_reflection
:chain
:conditions
:source_macro
:inverse_of
:polymorphic_inverse_of
:association_class

:build_association
:reset_column_information
:check_validity!
:check_validity_of_inverse!

:nested?
:has_inverse?
:collection?
:validate?
:belongs_to?
```

举例：可选参数 :inverse_of 可与哪些关联或不可与哪些可选参数一起使用。

```
VALID_AUTOMATIC_INVERSE_MACROS = [:has_many, :has_one, :belongs_to]
INVALID_AUTOMATIC_INVERSE_OPTIONS = [:conditions, :through, :polymorphic, :foreign_key]
```

另，HasManyReflection、 HasOneReflection、BelongsToReflection 和 HasAndBelongsToManyReflection 都继承于 AssociationReflection. 它们这几个方法比较少，就不再一一列举。

### ThroughReflection

另一个值得一提的就是：ThroughReflection. ThroughReflection 继承于 AbstractReflection，有下列方法：

```ruby
:foreign_key
:foreign_type
:association_foreign_key
:active_record_primary_key
:type
:source_reflection
:through_reflection
:chain
:conditions
:source_macro
:association_primary_key
:source_reflection_names
:source_options
:through_options

:check_validity!

:nested?
```

### 其它

Reflection 虽然很重要，但对于普通 Web 开发者而言，使用场景有限，一般不会直接使用。下面是我想到的一些使用场景，供参考：

1. 动态创建其关联对象的实例，如：在表单里点击按钮，创建一个嵌套对象(属性)。
2. 查看使用 gem 后引进了什么关联
3. 删除某个重要对象时，删除所有与之关联的对象。预防用 :dependent 或手动删除会有遗漏。
