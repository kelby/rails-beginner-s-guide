# 数据库操作之 Relation

Ruby --翻译--> SQL语句 --执行--> 返回结果

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

取数据，并**统计**，有一点偏向数学：

> Note: 它们都是直接返回结果。

## ~~Delegation~~

## FinderMethods

```
exists?

find, find_by, find_by!

first, first!
fifth, fifth!
fourth, fourth!
second, second!
third, third!
forty_two, forty_two!
last, last!

find_last, find_nth, find_nth_with_limit, find_one, find_some, find_take, find_with_ids

take, take!
```

## ~~HashMerger & Merger~~

## ~~PredicateBuilder~~

## QueryMethods

单独另讲

## SpawnMethods

| 方法 | 解释 |
| -- | -- |
| except | 查询方法有多个，并且可以链式调用。使用 except 忽略之前的某个查询方法 |
| merge | 参数是 Relation, 则做为查询条件，返回仍然是 Relation；参数是数组, 则返回前者的查询结果和此数组的交集 |
| only | 查询方法有多个，并且可以链式调用。使用 only 指定只能使用的查询方法 |

当参数是 Relation 时，merge 也和 joins、includes 等一样有联合查询的效果。
