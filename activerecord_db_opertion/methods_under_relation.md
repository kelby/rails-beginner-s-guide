## Relation 文件下的方法

| 方法 | 解释 |
| -- | -- |
| == | 判断集合是否等价于 relation. <br>这里的集合，并不限于 relation，也可以数组 |
| any? | 非空？ |
| empty? |为空？ |
| blank? | 为空？ |
| many? | 多个？<br> 也就是 relation.count > 1 |
| build & new | 新建 record 对象 |
| create | 新建并保存 record 对象 |
| create! | 新建并保存 record 对象，如果有异常则报错 |
| delete | 基于 SQL 删除指定 id 的 record 对象 |
| delete_all | 基于 SQL 删除 relation 包含的所有 record 对象<br>可以先把这些 record 对象查询出来，也可以删除的时候传条件进行查询|
| destroy | 删除指定 id 的 record 对象，会运行回调函数 |
| destroy_all | 删除 relation 包含的所有 record 对象，会运行回调函数 |
| explain | 1. 模拟在各自数据下的执行效果<br>2. 解释生成的 sql|
| find_or_create_by | 根据所给的属性，查询 record 对象；若查询不到，则创建 |
| find_or_create_by! | 根据所给的属性，查询 record 对象；若查询不到，则创建。创建则有可能失败，失败报错 |
| find_or_initialize_by |  |
| find_or_create_by! | 根据所给的属性，查询 record 对象；若查询不到，则初始化 |
| reset | 如果对所得的 relation 进行了处理，但还没有保存，使用它可重置之前的处理 |
| load | 执行查询操作，但返回的是 relation |
| update | 传递参数 ids、attributes，所有指定 id 的 record 对象都会更新<br>用的是自己的 update 方法 |
| update_all | 更新整个 relation 包含的 record 对象(先查询出来)，参数为更新内容 |
| values | 返回规格化的所有查询条件。如果你用的查询条件太多了，可用它来查看 |
| where_values_hash | 返回所有 where 查询条件，结果并不准确 |
| reload | reset + load |
| cache_key | ... |
| none? | ... |
| one? | ... |

和


除上述方法外，还有：

```
to_a

to_sql
```

一般，我们不需要也不推荐使用 `to_a` 强制转换 Relation 成数组；使用 `to_sql` 可以方便的查看生成的 Sql 语句。

```
eager_loading?

encode_with

initialize_copy

inspect

joined_includes_values

pretty_print

scope_for_create

scoping

size

uniq_value
```
