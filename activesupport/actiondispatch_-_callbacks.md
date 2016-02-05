## Callback 方法解释及使用

这里是 Rails 所有回调的源头。从表面看，它提供以下方法。

类方法：

```
define_callbacks

set_callback
skip_callback

reset_callbacks
```

实例方法：

```
run_callbacks
```

已有回调类型：

```ruby
CALLBACK_FILTER_TYPES = [:before, :after, :around]
```

使用过程中，3 个必不可少的方法及其解释如下：

| 方法 | 解释 |
| -- | -- |
| define_callbacks | 定义一条"回调链"。 |
| set_callback | 把某种类型的"回调"放到"回调链"里。<br>需要 3 个参数：回调链的名字、回调的类型(不传递的话，自动使用 :before)、回调的名字。 |
| run_callbacks | 在执行代码的前后，执行指定"回调链"相关的"回调"。<br>第一个参数是"回调链"的名字，第二个参数是个 block，里面放真正要执行的代码。|

使用举例：

```ruby
class Report
  include ActiveSupport::Callbacks

  # 定义一条回调链，名字是 print
  define_callbacks :print
  
  # 把类型为 before，名字为 before_print 的回调，放到 print 回调链里
  set_callback :print, :before, :before_print
  # 把类型为 after，名字为 after_print 的回调，放到 print 回调链里
  set_callback :print, :after, :after_print
  
  def print_me
    # 在执行"真正要执行的代码"的前后，执行 print 回调链里的回调
    run_callbacks :print do
      # 真正要执行的代码
      p 'print me'
    end
  end

  # 以下两方法已经放入 print 回调链，所以会被调用。

  def before_print
    p 'before print'
  end

  def after_print
    p 'after print'
  end
end

Report.new.print_me
# => "before print"
# => "print me"
# => "after print"
```
