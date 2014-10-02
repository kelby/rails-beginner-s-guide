# ActiveRecord Migrator Others
本身实现了：

```
up()
down()
migrate()
```

它下面的 **CommandRecorder** 实现了：

```
add_column
add_index
add_reference
add_timestamps

create_table
create_join_table

drop_table (must supply a block)
drop_join_table (must supply a block)

remove_timestamps
remove_reference
remove_column
remove_columns
remove_index

rename_column
rename_index
rename_table

change_column_default
change_column
change_column_null
change_table

transaction
execute
execute_block
enable_extension
```

[Active Record Migrations](http://edgeguides.rubyonrails.org/migrations.html)

## 迁移相关(migrate目录、schema.rb文件)

Migration 提供的：

```ruby
up
down

migrate(direction)
revert(*migration_classes)
```

SchemaMigration 提供的：

```ruby
create_table
drop_table
```

引用：

```
bulk參數

:bulk => true 可以讓變更資料庫欄位的 Migration 更有效率的執行，如果沒有加這個參數，或是直接使用 add_column、rename_column、remove_column 等方法，那麼 Rails 會拆開 SQL 來執行，例如：

change_table(:users) do |t|
  t.string :company_name
  t.change :birthdate, :datetime
end

會產生：

ALTER TABLE `users` ADD `im_handle` varchar(255)
ALTER TABLE `users` ADD `company_id` int(11)
ALTER TABLE `users` CHANGE `updated_at` `updated_at` datetime DEFAULT NULL

加上:bulk => true之後：

change_table(:users, :bulk => true) do |t|
  t.string :company_name
  t.change :birthdate, :datetime
end

會合併產生一行SQL：

ALTER TABLE `users` ADD COLUMN `im_handle` varchar(255), ADD COLUMN `company_id` int(11), CHANGE `updated_at` `updated_at` datetime DEFAULT NULL

這對已有不少資料量的資料庫來說，會有不少執行速度上的差異，可以減少資料庫因為修改被 Lock 鎖定的時間。
```
CommandRecorder 提供的：

```ruby
:create_table, :create_join_table, :rename_table, :add_column, :remove_column,
:rename_index, :rename_column, :add_index, :remove_index, :add_timestamps, :remove_timestamps,
:change_column_default, :add_reference, :remove_reference, :transaction,
:drop_join_table, :drop_table, :execute_block, :enable_extension,
:change_column, :execute, :remove_columns, :change_column_null
及 :change_table
```

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
