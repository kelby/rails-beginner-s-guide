### 对比，然后使用合适的方法

- delete vs destroy

前者不会触发回调，后者会。前者速度更快。

- delete_all vs destroy_all

前者会触发回调，后者不会。前者速度更快。

- update_column vs update_attribute

前者不会触发校验，后者会。前者速度更快。

- update_column/update_attribute vs save

前者更新部分，后者更新所有(并且要运行校验、回调)。前者速度更快。

- update_all/find_in_batches vs for/each

前者为批量操作，后者不是。前者速度更快。

- select vs 默认查询

前者指定查询部分字段，后者查询所有字段。前者速度更快。

- pluck and plucks vs map

前者直接指定查询部分字段，后者查询所有字段然后才取部分字段。前者速度更快。

- index vs 默认查询

前者借助了索引，后者没有。前者速度更快。

- 原生 SQL vs AR 方法

使用原生 SQL 比使用 ActiveRecord 方法要快。(不解释)

参考

[Rails-with-massive-data](http://blog.xdite.net/posts/2012/08/22/rails-with-massive-data)
