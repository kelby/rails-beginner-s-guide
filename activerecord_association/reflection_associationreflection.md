### ~~Association Reflection~~

Association Reflection 继承于 Macro Reflection 又继承于 Abstract Reflection

```ruby
klass
compute_class
type
foreign_type

parent_reflection
parent_reflection=

association_scope_cache
constructable?
join_table
foreign_key
association_foreign_key
association_primary_key
active_record_primary_key
counter_cache_column
check_validity!
check_validity_of_inverse!
check_preloadable! & check_eager_loadable!
join_id_for
through_reflection
source_reflection
chain
nested?
scope_chain
has_inverse?
inverse_of
polymorphic_inverse_of
macro
collection?
validate?
belongs_to?
has_one?
association_class
polymorphic?
```

举例：可选参数 :inverse_of 可与哪些关联或不可与哪些可选参数一起使用。

```
VALID_AUTOMATIC_INVERSE_MACROS = [:has_many, :has_one, :belongs_to]
INVALID_AUTOMATIC_INVERSE_OPTIONS = [:conditions, :through, :polymorphic, :foreign_key]
```

另，HasManyReflection、 HasOneReflection、BelongsToReflection 和 HasAndBelongsToManyReflection 都继承于 AssociationReflection. 它们这几个方法比较少，就不再一一列举。
