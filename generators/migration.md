## Migration

require 'actions/create_migration'

```
create_migration
migration_template
set_migration_assigns!
```

`migration_template` 和 template、copy_file 类似，复制模板生成新的文件。但不同点在于，这里新生成的文件会自动加上时间戳。
