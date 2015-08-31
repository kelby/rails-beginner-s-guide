### ~~Alias Tracker~~

**作用**：为 Join Dependency 和 Through Association Scope 服务。

实现对不规则表名的跟踪及相关处理。

实例方法：

```
attr_reader :aliases, :connection

aliased_table_for
```

类方法：

```
empty

create

initial_count_for
```
