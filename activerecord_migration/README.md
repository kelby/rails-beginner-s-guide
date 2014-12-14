# Active Record 迁移

- Migration

- Schema

- Connection Adapters

下面这 4 个模块，来源 Connection Adapters，它下面还有很多模块，在此忽略其它 ...

包括：Schema Statements、Table Definition、Table 和 Database Statements.

- 其它

包括：Model Schema*、~~Schema Migration~~ 和 Command Recorder.

`ActiveRecord::Base.connection.table_exists? 'kittens'`
