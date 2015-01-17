## Database Tasks

rake db:migrate 等迁移命令，定义在 Active Record 的 databases.rake 里。
其中，大部分命令都是由 ActiveRecord::Tasks::DatabaseTasks 来处理，再或者转发到其它模块。

提供接口如下：

```
create_current    # 对应 rake db:create
create_all        # 对应 rake db:create:all

drop_current      # 对应 rake db:drop
drop_all          # 对应 rake db:drop:all

purge_current     # 对应 rake db:purge
purge_all         # 对应 rake db:purge:all

migrate           # 对应 rake db:migrate

charset_current   # 对应 rake db:charset
collation_current # 对应 rake db:collation
```

其它接口，不在此一一列举：

```
charset
check_schema_file
collation
create
current_config
db_dir
drop
env
fixtures_path
load_schema, load_schema_current
load_seed
migrations_paths
purge
register_task
root
seed_loader
structure_dump, structure_load
```
