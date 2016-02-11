### Association Relation

针对 Scope `merge` 操作的实现：

```ruby
# 可以被重写(ThroughAssociation 里就这么做)
def target_scope
  AssociationRelation.create(klass, klass.arel_table, klass.predicate_builder, self).merge!(klass.all)
end
```
