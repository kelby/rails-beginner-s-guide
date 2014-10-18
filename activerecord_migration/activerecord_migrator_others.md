# ActiveRecord Migrator Others

[Active Record Migrations](http://edgeguides.rubyonrails.org/migrations.html)

**更高效的迁移**

有 update_all 可以用，少用 for / each
不要傻傻的直接 Post.all.each，可以用 find_in_batches

使用 transaction 跳過每次都要 BEGIN COMMIT 的過程，一次做完 1000 筆，然後再 COMMIT

使用 update_column / sneacky-save 而非原生 save

可以的話使用 Post.select("column 1, colum2").where （不要 posts.map(&:id)，而是posts.select(:id).map(&:id)，或者用pluck）

使用 delegate 把大資料搬出去

操作資料前，別忘記打 INDEX

delete / destroy，刪除很昂貴。確保你知道自己在幹什麼。

[Rails-with-massive-data](http://blog.xdite.net/posts/2012/08/22/rails-with-massive-data)

数据库操作这块，理清头绪：读、写？目标结果是单个对象、多个对象？进行操作的是单个对象、多个对象？

[Rails SQL Injection](http://rails-sqli.org/)  
[Active Record Queries](http://www.theodinproject.com/ruby-on-rails/active-record-queries)
