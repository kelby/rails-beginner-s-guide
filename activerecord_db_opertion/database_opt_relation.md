## Relation(Arel)

对 Relation 的操作。哦，我们先理解概念。

- 为什么不完全用 Ruby 或 SQL？

Ruby 慢，人性化；SQL 快，不易读写。

- 如何充分利用两者优点，特别是我们要做复杂查询的时候？(复杂查询 = 简单的查询 + 简单的查询 + ...)

"(Ruby 查询 -> SQL 查询，Ruby 查询 -> SQL 查询) -> 结果" 比 "(Ruby 查询 + Ruby 查询 -> SQL 查询) -> 结果" 效率要低。所以尽量不要在 Ruby - SQL 两个层面之间转来转去，尽量把多个 Ruby 查询转化成相应的一个 SQL 查询。

- 如何实现？

运用中间状态，也就是 Relation. 每次查询并不是真正的查询(因为没走到 SQL 层面)，而是保存一个中间状态，当你所有的查询条件都写完了，才进入 SQL 的层面。理论上，这些简单的查询最后都能组合成 SQL 语句。

- 好处？

延迟加载。我们在 Controller 里有一个查询语句，结果赋值给一个实例变量，原本的意图是在 View 里显示的。某天需求更改了，我们不必再显示这个查询结果，但 Controller 里我们忘记删除这部分的代码(这都能忘？)，结果每次都要做大量无用的查询工作。引入中间状态后，就能起到延迟加载的作用，不到最后的调用，不做 SQL 查询(停留在 Ruby 层面)。

链式查询，效率高。上面已经提到了"(Ruby 查询 -> SQL 查询，Ruby 查询 -> SQL 查询) -> 结果" 比 "(Ruby 查询 + Ruby 查询 -> SQL 查询) -> 结果" 效率要低。

- 如何区分？

在控制台里执行一下查询命令，看返回结果的 class (类型)。有 ActiveRecord::Relation、 ActiveRecord::AssociationRelation 和 ClassName::ActiveRecord_Relation 等包含 Relation 字样的就是了。

Relation 就类似没有名字的 scope 。当涉及跨表查询时，使用链式查询可以很大程度的提高效率。更多请查看接口 [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)

> Note: 这里部分是对多个对象的操作，对 Relation 的操作；不是查询操作。
