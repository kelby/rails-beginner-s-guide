### 控制台里迁移、回滚等命令

主要命令

```
rake db:migrate
rake db:rollback

rake db:migrate:up
rake db:migrate:down

rake db:migrate:redo
```

指定版本号的回滚

```
rake db:migrate:down VERSION=20141119130134
```

回滚最近几个迁移

```
rake db:rollback STEP=n
```

n 代表个数。注意：是最近几个，它们会被一起移除。

其它类似命令：

- 只执行指定版本号的迁移

```
rake db:migrate VERSION=20141119130134
```

- 只执行最近几次迁移

```
rake db:migrate STEP=n
```

- 回滚、然后重新执行最近几次迁移

```
rake db:migrate:redo STEP=n
```
