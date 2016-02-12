## 11 个 Association 文件 - 核心之提供方法

**关联方法带来的高级的方法。**

```
HasOneThroughAssociation BelongsToPolymorphicAssociation  HasManyThroughAssociation
   |                      |                                 |
   V                      V                                 V
  HasOne   &         BelongsTo               HasMany
           |                                    |
           V                                    V
    SingularAssociation      &     CollectionAssociation
                             |
                             V   
                         Association

# 其它：
ForeignAssociation
ThroughAssociation
```

**对外提供接口。**

概念：a.b 或 a.bs 组成一个 Association，把它们看成是一个整体。但前者为 owner，后者为 target.

实现对关联对象的操作。

