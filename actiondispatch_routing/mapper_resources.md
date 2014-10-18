### Resources

有选择的讲：

```ruby
collection() - 嵌套于 resources
member() - 嵌套于 resources
resource(*resources, &block) - 对应的是单数
resources(*resources, &block) - 对应的是复数
```

```
match - 同名
namespace - 同名
root - 同名
```

VALID_ON_OPTIONS  = [:new, :collection, :member]
RESOURCE_OPTIONS  = [:as, :controller, :path, :only, :except, :param, :concerns]
