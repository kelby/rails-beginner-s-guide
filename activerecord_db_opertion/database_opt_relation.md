# 数据库操作之 Relation

Ruby -> SQL

至少两个过程：翻译、执行。

Relation 就是这个翻译过程。

翻译一句，执行一句；
还是全部翻译完了，再一次执行。

使用 Relation 就相当于选择了后者。

## Batches

```
find_each
find_in_batches
```

一般，find_each 后面参数都是 block，此时作用的 find_in_batches 类似(此时，batch_size 用的是默认 1000)

## Calculations

```
count   # 总数
average # 平均值
maximum # 最大值
minimum # 最小值
sum     # 值的总和

pluck # 攫取指定属性
ids   # 攫取 id 属性

calculate
```

## Delegation

## FinderMethods

## HashMerger & Merger

## PredicateBuilder

## QueryMethods

## SpawnMethods
