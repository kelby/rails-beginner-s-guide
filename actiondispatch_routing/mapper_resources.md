### Resources

常用方法：

| 方法 | 解释 |
| -- | -- |
| resource | 我们的资源不需要集合操作的时候可以使用 |
| resources | 我们的资源需要集合操作的时候可以使用，用得最多 |
| collection | 嵌套于 resources，里面跟的是集合 |
| member | 嵌套于 resources，里面跟的是单个元素 |
| match | 嵌套于 resources，不再解释 |
| root | 嵌套于 resources，不再解释 |
| namespace | 嵌套于 resources，不再解释 |

其中

```
VALID_ON_OPTIONS = [:new, :collection, :member]
RESOURCE_OPTIONS = [:as, :controller, :path, :only, :except, :param, :concerns]
```

其它方法(不再介绍)：

```
nested

new

resources_path_names

shallow
shallow?

using_match_shorthand?
```

和

```
set_member_mappings_for_resource

with_exclusive_scope

with_scope_level
```
