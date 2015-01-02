## Associations 架构图讲解

### 1) 4 个关联类方法：

对外 API:

```
belongs_to

has_and_belongs_to_many

has_many

has_one
```

方法本身表示什么意思。

引进了哪些方法，表示什么意思。

有什么参数，表示什么意思，使用后有什么效果。

注意事项。

### 2) Builder

影响对外表现，而又不涉及枯燥的原理。

直接对应上面 4 个方法的有：

```
HasMany

HasOne

BelongsTo

HasAndBelongsToMany
```

想更好的理解和使用 Association 提供的 4 个方法，强烈推荐阅读这里的代码。它所影响的是对外表现，包括：引进了哪些方法，表示什么意思；有什么参数，表示什么意思，使用后有什么效果。

对比查看，还能知道各个关联之间有什么异同点。

```
  HasOne   &   BelongsTo        HasMany   &   HasAndBelongsToMany
           |                              |
           V                              V
    SingularAssociation      &     CollectionAssociation
                             |
                             V   
                         Association
```

### 3) Association

内部实现，如同名模块：

```
Association

SingularAssociation

CollectionAssociation
```

### 4) through 参数

### 5) 对象.关联对象 操作

### 6) 关联对象的 scope 参数(或查询)

### 7) join 方法

### 8) 违背约定
