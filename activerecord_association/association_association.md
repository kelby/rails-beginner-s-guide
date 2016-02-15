#### ~~1) Association~~

```ruby
attr_reader :owner, :target, :reflection
attr_accessor :inversed

delegate :options, :to => :reflection
```

```
aliased_table_name

reset

reload
loaded?
loaded!

stale_target?

target=

scope

association_scope

reset_scope

set_inverse_instance

klass

target_scope

load_target

interpolate

marshal_dump
marshal_load

initialize_attributes
```

调用到了 AssociationScope、AssociationRelation 等类。

