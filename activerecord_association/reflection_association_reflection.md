#### ~~Association Reflection~~

Association Reflection 继承于 Macro Reflection 又继承于 Abstract Reflection

```ruby
klass
compute_class

attr_reader :type, :foreign_type
attr_accessor :parent_reflection

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

scope_chain
inverse_of
polymorphic_inverse_of
macro
association_class

nested?
has_inverse?
collection?
validate?
belongs_to?
has_one?
polymorphic?
```

举例：可选参数 :inverse_of 可与哪些关联或不可与哪些可选参数一起使用。

```
VALID_AUTOMATIC_INVERSE_MACROS = [:has_many, :has_one, :belongs_to]
INVALID_AUTOMATIC_INVERSE_OPTIONS = [:conditions, :through, :polymorphic, :foreign_key]
```

另，Has Many Reflection、 Has One Reflection、Belongs To Reflection 和 Has And Belongs To Many Reflection 都继承于 Association Reflection. 它们这几个方法比较少，就不再一一列举。
