# Associations 架构图

    Associations 文件下
      4 个关联类方法 -- (1)

    本目录下 -- 实现几个关联方法。
      builder -- (2)
        Association
          SingularAssociation
            HasOne
            BelongsTo
          CollectionAssociation
            HasMany
        HasAndBelongsToMany
        4 个关联类方法，直接调用

      # 10 个 _Association 文件。实现对关联对象的操作！
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
            HasManyThrough
              include ThroughAssociation
            HasMany
          SingularAssociation
            BelongsTo
            HasOne
            HasOneThrough
              include ThroughAssociation
