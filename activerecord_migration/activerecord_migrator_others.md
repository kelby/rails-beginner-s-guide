## Migrator 其它

**更高效的迁移**

能批量操作的尽量批量操作，如：

用 update_all/find_in_batches 而不用 for/each

能用 SQL 层面的尽量用，如：
用 update_column 而不用 save

可以的話使用 Post.select("column 1, colum2").where （不要 posts.map(&:id)，而是posts.select(:id).map(&:id)，或者用pluck）

使用 delegate 把大資料搬出去，避免操作不必要的资料。

操作資料前，別忘記打 INDEX

delete / destroy，刪除很昂貴。確保你知道自己在幹什麼。

[Rails-with-massive-data](http://blog.xdite.net/posts/2012/08/22/rails-with-massive-data)

数据库操作这块，理清头绪：读、写？目标结果是单个对象、多个对象？进行操作的是单个对象、多个对象？
