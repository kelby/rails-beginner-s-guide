##其它


### Integration

### Enum

### CounterCache

### Callbacks

### Aggregations

### Locking

### AttributeMethods

太多了，真正涉及的时候再讲吧。

## 场外

array 是引用对象
[1, 2, 3] 是值对象

Relation 就相当于引用对象。

引进 Relation 概念，主要就是为了充分利用 SQL 层面的东西(读写)，毕竟执行 SQL 比执行 Ruby 代码速度(效率)更高。但语法用的还是 Ruby 这一套，毕竟它更友好(符合人类阅读习惯)。

----------

ActiveRecord 里设置表名、列名除了就对不规则外，还有一个功能是应对遗留数据库。如：
table_name_prefix
primary_key_prefix_type
pluralize_table_names
primary_key
等
