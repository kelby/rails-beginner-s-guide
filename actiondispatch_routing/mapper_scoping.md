### Scoping

有选择的讲：

| 方法 | 解释 |
| -- | -- |
| scope | 当几个路由规则的可选参数(部分)都一样的时候，可以用`scope`把它们包住，避免重复 |
| namespace | 基于 scope，只是设置了 :module => path |
| controller | 基于 scope，只是设置了 :controller => controller |
| constraints | 基于 scope，只是设置了 :constraints |
| defaults | 基于 scope，只是设置了 :defaults |

**比较 namespace 和 scope**

|            | namespace | scope(第一个参数是字符串)  |
| :--------: | :--------:| :--:   |
| 同         | 默认在 url 里有前缀   |  默认在 url 里有前缀   |
| 异         |  默认Controller有命名空间 |  默认Controller无命名空间  |

scope 可选参数：
```ruby
SCOPE_OPTIONS = [:path, :shallow_path, :as, :shallow_prefix, :module,
                       :controller, :action, :path_names, :constraints,
                       :shallow, :blocks, :defaults, :options]
```
