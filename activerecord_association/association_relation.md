### Association Relation

针对 Scope `merge` 操作的实现：

```ruby
# Can be overridden (i.e. in ThroughAssociation) to merge in other scopes (i.e. the
# through association's scope)
def target_scope
  AssociationRelation.create(klass, klass.arel_table, klass.predicate_builder, self).merge!(klass.all)
end
```
