## Schema Migration

重要的如：

```
create_table
drop_table
```

其它

```
index_name
normalize_migration_number
normalized_versions
primary_key
table_exists?
table_name
version
```

## Schema

继承于 Migration

常用的如：

```
define
```

对应的是 db/schema.rb 里的方法，举例：

```ruby
ActiveRecord::Schema.define(version: 20380119000001) do
  ...
end
```

其它方法：

```
migrations_paths
```

