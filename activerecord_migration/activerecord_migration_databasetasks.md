# DatabaseTasks

ActiveRecord::Tasks::DatabaseTasks is a utility class, which encapsulates logic behind common tasks used to manage database and migrations.

The tasks defined here are used with Rake tasks provided by Active Record.

In order to use DatabaseTasks, a few config values need to be set. All the needed config values are set by Rails already, so it's necessary to do it only if you want to change the defaults or when you want to use Active Record outside of Rails (in such case after configuring the database tasks, you can also use the rake tasks defined in Active Record).

The possible config values are:

* +env+: current environment (like Rails.env).
* +database_configuration+: configuration of your databases (as in +config/database.yml+).
* +db_dir+: your +db+ directory.
* +fixtures_path+: a path to fixtures directory.
* +migrations_paths+: a list of paths to directories with migrations.
* +seed_loader+: an object which will load seeds, it needs to respond to the +load_seed+ method.
* +root+: a path to the root of the application.

Example usage of DatabaseTasks outside Rails could look as such:

```ruby
include ActiveRecord::Tasks
DatabaseTasks.database_configuration = YAML.load_file('my_database_config.yml')
DatabaseTasks.db_dir = 'db'
# other settings...

DatabaseTasks.create_current('production')
```

提供接口如下：

```
charset, charset_current, check_schema_file, collation, collation_current, create, create_all, create_current, current_config
db_dir, drop, drop_all, drop_current
env
fixtures_path
load_schema, load_schema_current, load_seed
migrate, migrations_paths
purge, purge_all, purge_current
register_task, root
seed_loader, structure_dump, structure_load
```
