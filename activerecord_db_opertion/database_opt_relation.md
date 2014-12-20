由以下几个部分组成。

## 概念

Ruby --翻译--> SQL语句 --执行--> 返回结果。

至少两个过程：翻译、执行。

Relation 就是这个翻译过程。

翻译一句，执行一句；
还是全部翻译完了，再一次执行。

使用 Relation 就相当于选择了后者。

## ~~HashMerger & Merger~~

是 Spawn Methods 里的 merge、merge! 方法的底层实现。

在 merger 对象非 Array、非 Relation、非 proc 等情况下才使用到。

## ~~Delegation~~

## ~~Predicate Builder~~

## Finder Methods & Batches 补充

查表操作(数据库读操作)。大部分是SQL层面，一般不可多条件链式查询。

> Note: 返回的都是结果，不是 Relation。
