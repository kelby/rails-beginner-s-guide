## Callbacks 介绍

`define_callbacks(*names)`
定义多个 callback，但不能传递类型，只能使用默认的，也就是 before 类型。基于 `set_callback`

会有 "_#{name}_callbacks"

```ruby
define_callbacks :validate
define_callbacks :initialize, :save, :destroy
```

`set_callback(name, *filter_list, &block)` 是调用，不是设置！
第二个参数 filter_list 只能是 :before, :after, 或 :around 里的一个或多个，如果不设置，默认是 :before

```ruby
# before save 执行 before_meth
set_callback :save, :before, :before_meth

# 如果满足条件，在 after save 执行 after_meth
set_callback :save, :after,  :after_meth, if: :condition

# around save 始终执行后面的 lambda
set_callback :save, :around, ->(r, &block) { stuff; result = block.call; stuff }
```

除了上述方法外，还有：

```ruby
reset_callbacks(name)
skip_callback(name, *filter_list, &block)
```

上面给的方法，无论是定义还是调用都是通过名字的，如果你不想要名字，直接调用 `run_callbacks(kind, &block)`

> 使用举例：AbstractController::Callbacks 和 ActiveModel::Callbacks，在它们的章节会有介绍。
