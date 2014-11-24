## Reflection

一个很重要的概念，包含了所有的关联信息。
包括但不限于：用的是什么关联、关联对象名字、可选参数等。

对于**一般关联和 aggregate 要区分**开来。前者用 _reflections，后者用 aggregate_reflections.

提供方法

### self

```
create
可以创建 AggregateReflection，HasManyReflection、HasOneReflection 和 BelongsToReflection 4 种关联
如果使用了 :through 则还会自动生成 ThroughReflection 关联

add_reflection
add_aggregate_reflection
```

### Class Methods

```
reflections # 所有正常的关联
reflect_on_all_associations          # 指定 macro 的 reflections
reflect_on_all_autosave_associations # 包含 autosave 的 reflections

reflect_on_all_aggregations # 所有 aggregate 关联

# 以下两方法要提供被关联对象名字
reflect_on_association
reflect_on_aggregation
```

### 结构图

```
AbstractReflection
  MacroReflection
    AssociationReflection
      AggregateReflection
      HasManyReflection
      HasOneReflection
      BelongsToReflection
        ThroughReflection
```

### 其它

Reflection 虽然很重要，但对于普通 Web 开发者而言，使用场景有限，一般不会直接使用。下面是我想到的一些使用场景，供参考：

1. 动态创建其关联对象的实例，如：在表单里点击按钮，创建一个嵌套对象(属性)。
2. 查看使用 gem 后引进了什么关联
3. 删除某个重要对象时，删除所有与之关联的对象。预防用 :dependent 或手动删除会有遗漏。
