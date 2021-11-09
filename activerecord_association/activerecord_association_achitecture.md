# Associations 数据库表的关联关系

```bash
Associations 文件下
    4 个关联类方法 -- (1)

Associations 目录下 -- 实现几个关联方法。
    # 7 个 Builder 文件
    builder -- (2)
        HasAndBelongsToMany
        Association
            SingularAssociation
                HasOne
                BelongsTo
            CollectionAssociation
                HasMany
        4 个关联类方法，直接调用它们

    # 11 个 _Association 文件。实现对关联对象的操作！
    Association -- (3)
        SingularAssociation --(3)
            HasOneAssociation
                HasOneThroughAssociation
                    include ThroughAssociation
            BelongsToAssociation
                BelongsToPolymorphicAssociation
        CollectionAssociation --(3)
            HasManyAssociation
                HasManyThroughAssociation
                    include ThroughAssociation
    ThroughAssociation -> HasOneThroughAssociation + HasManyThroughAssociation
    ForeignAssociation -> HasOneAssociation + HasManyAssociation

    # 其它

    CollectionProxy(*) -> CollectionAssociation
        继承于 Relation

    AssociationScope -> CollectionAssociation + SingularAssociation + Association

    JoinDependency(实现 joins) -> FinderMethods + QueryMethods
        JoinPart
            JoinBase
            JoinAssociation

    AliasTracker -> AssociationScope + JoinDependency

    Preloader --(实现 includes, preload, eager_load)
        ThroughAssociation
        Association
            CollectionAssociation
                HasMany
                HasManyThrough
                    include ThroughAssociation
            SingularAssociation
                BelongsTo
                HasOne
                HasOneThrough
                    include ThroughAssociation

Aggregations

AutosaveAssociation

# Reflection 文件
AbstractReflection
    ThroughReflection
        PolymorphicReflection
    MacroReflection
        AggregateReflection
        AssociationReflection
            HasManyReflection
            HasOneReflection
            BelongsToReflection
            HasAndBelongsToManyReflection
```



