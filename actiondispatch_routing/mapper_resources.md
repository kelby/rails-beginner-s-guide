### Resources

常用方法：

| 方法 | 解释 |
| -- | -- |
| collection | 嵌套于 resources, 里面跟的是集合 |
| member | 嵌套于 resources，里面跟的是单个元素 |
| resource | 我们的资源不需要集合操作的时候可以使用 |
| resources | 我们的资源需要集合操作的时候可以使用，用得最多 |

其它同名方法，不再介绍：

```
match
member
root
namespace
```

其它：

```
VALID_ON_OPTIONS  = [:new, :collection, :member]
RESOURCE_OPTIONS  = [:as, :controller, :path, :only, :except, :param, :concerns]
```
