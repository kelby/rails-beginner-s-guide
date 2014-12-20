## Generator

**url_for 方法的重要组成部分。**

各个实例方法：

```
generate

# 以下几个方法 initialize 时被调用
normalize_recall!
normalize_options!
normalize_controller_action_id!
use_relative_controller!
normalize_controller!
normalize_action!
```

initialize 和 `generate` 对外提供的接口。

除上述外，还有：

```
use_recall_for

controller
current_controller

different_controller?
```

和

```
attr_reader :options, :recall, :set, :named_route
```

