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

此外：

```ruby
VALID_ON_OPTIONS = [:new, :collection, :member]
RESOURCE_OPTIONS = [:as, :controller, :path, :only, :except, :param, :concerns]
```

其它方法：

```
nested

new

resources_path_names

shallow
shallow?

using_match_shorthand?
```
