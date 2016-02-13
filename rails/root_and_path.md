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

另，Path 元编程提供的几个方法，也挺有用的：

```ruby

%w(autoload_once eager_load autoload load_path).each do |m|
  class_eval <<-RUBY, __FILE__, __LINE__ + 1
    def #{m}!        # def eager_load!
      @#{m} = true   #   @eager_load = true
    end              # end
                     #
    def skip_#{m}!   # def skip_eager_load!
      @#{m} = false  #   @eager_load = false
    end              # end
                     #
    def #{m}?        # def eager_load?
      @#{m}          #   @eager_load
    end              # end
  RUBY
end
```

Path 行为和数组有点类似，并且它还 `include Enumerable`，所以部分操作也可用。

