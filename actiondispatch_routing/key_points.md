### match 和 scope 方法 - 重中之重

从前面各章节，可归纳出路由规则里， match 和 scope 两方法是重中之重。
<br>
其它方法，都在在这两方法的基础上，做封装、语法糖、去重复代码等。

`match` 是生成路由规则，`scope` 是影响如何生成路由规则。

和 `match` 有关的有：get, post, delete, put 等, 只有通过它们才能添加路由规则；
<br>
和 `scope` 有关的有：namescope, collection, member, resource, resouces 及嵌套路由时自动完成的 nested 等。它们主要工作是调用上面的 match 相关方法，并影响生成路由规则。

完成路由规则整个过程中，match 和 scope 缺一不可。
