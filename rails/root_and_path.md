## 路径 - Root 和 Path

包括 Root & Path，但 Root 已经封装了 Path, 并且只有 Root 对外提供接口。

```ruby
Rails.application.paths
```

提供方法：

```
add

all_paths

autoload_once
autoload_paths

eager_load

load_paths
```

和

```
keys
values
values_at

[]
[]=
```

这部分内容，偏底层了。
