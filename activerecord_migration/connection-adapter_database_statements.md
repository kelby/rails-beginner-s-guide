## Database Statements

常用的如：

```
execute
```

有时候，我们会在迁移文件里直接运行 SQL，使用上述方法，可以达到目的。

举例：

```ruby
class MakeJoinUnique < ActiveRecord::Migration
  def up
    execute "ALTER TABLE `pages_linked_pages` \n
             ADD UNIQUE `page_id_linked_page_id` (`page_id`,`linked_page_id`)"
  end

  def down
    execute "ALTER TABLE `pages_linked_pages` DROP INDEX `page_id_linked_page_id`"
  end
end
```

其它方法，不在此一一列举。

