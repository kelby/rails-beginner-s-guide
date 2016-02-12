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
