## Callbacks 底层简要分析

ActiveSupport::Callbacks 本身可分为几部分。

#### Callback Chain 回调链

`define_callbacks` 主要作用就是定义一条"回调链"，每一条链都是 Callback Chain 的实例对象。

Callback Chain 实例对象，大致如下：

```
#<ActiveSupport::Callbacks::CallbackChain:0x007fc6ebf70648
  @callbacks=nil, 
  @chain=[], 
  @config={:scope=>[:kind]}, 
  @mutex=#<Mutex:0x007fc6ebf70580>, 
  @name=:checkout>
```

其中，最重要的信息有：

- `@chain` 此回调链所包含的"所有回调"。

- `@name` 此回调链的"名字"。

Rails 没有提供查看所有 callback chain 信息的接口，只能间接查看。

比如：

```ruby
# 所有"回调链"
callback_chains = ObjectSpace.each_object(ActiveSupport::Callbacks::CallbackChain)
# 不为空的"回调链"
chains = callback_chains.select{|cc| cc.instance_variable_get("@chain").present? }

# 所有"回调链"的名字
callback_chains_uniq_name = chains.map(&:name).uniq
```

但对于某个类而言，则很方便。如：

```ruby
callback_chains = ClassName.singleton_methods.select do |method|
  method.to_s =~ /^_[^_].+_callbacks$/
end

# Rails model 默认有 callback chains：
:_save_callbacks
:_create_callbacks
:_update_callbacks
:_validate_callbacks
:_validation_callbacks
:_initialize_callbacks
:_find_callbacks
:_touch_callbacks
:_destroy_callbacks
:_commit_callbacks
:_rollback_callbacks

# 查看某 callback chain 详情。如：
User._save_callbacks
```

#### Callback 回调

上面提到每一条回调链的 @chain 里包含了它"所有的回调"，这里的每一个"回调"就是一个 Callback 实例对象。

Callback 实例对象，大致如下：

```ruby
#<ActiveSupport::Callbacks::Callback:0x007fc6f2162248
  @chain_config={:scope=>[:kind]},
  @filter=#<Proc:0x007fc6f2162388@/Users/.../lib/rails/application/finisher.rb:100>,
  @if=[],
  @key=70246220894660,
  @kind=:before,
  @name=:prepare,
  @unless=[]
```

其中，最重要的信息有：

- `@if` 或 `@unless` 此回调起作用的"前提条件"。

- `@kind` 此回调的"回调类型"。(对于 Rails 而言，可以选择 :before, :after, :around 其中之一)

- `@name` 此回调所加入的"回调链的名字"。

- `@key` 此回调的"名字"。(如果没有名字则用 object_id 代替)

- `@filter` 此回调"真正要执行的代码"。(一般只显示名字，也就是说和 @key 一样；如果没有名字，则显示定义它时的文件及行号)

"真正要执行的代码"，可以是以下类型:

```
Symbols:: 方法名。
Strings:: 可求值的字符串。
Procs::   proc 对象。
Objects:: 普通的实例对象，但必需有 before_x, around_x, after_x 等"回调"方法。
          因为，之后会以拼接字符串的形式，找出对应的回调方法，然后 send 调用。

其实，还有一种类型 Conditionals 但被直接跳过了，所以不算在内。
```

这些不同类型的"真正要执行的代码"，之后都会被 Callback 的 `make_lambda` 方法转换成 lambda 对象，再然后处理过程类似。

#### 其它

**当 @filter 是一个实例对象，并且恰好有和"回调类型"相同的方法**

上面也提到"回调类型"，Rails 默认有 :before、:after 和 :around.
<br>
上面提到了回调里"真正要执行的代码"可以是一个实例对象，当触发回调时，会执行它的同名"回调"方法。

如果 @filter 恰好是一个实例对象，而这个实例对象又恰好有 :before、:after 或 :around，则此时会出现和想像中不一样的结果。

比如：

```ruby
require 'active_support'

class Audit
  def before(caller)
    puts 'Audit: before is called'
  end

  def before_save(caller)
    puts 'Audit: before_save is called'
  end
end

class Account
  include ActiveSupport::Callbacks

  define_callbacks :save
  set_callback :save, :before, Audit.new

  def save
    run_callbacks :save do
      puts 'save in main'
    end
  end
end

Account.new.save
```

此时，我们可以使用 `define_callbacks` 的 scope 参数进行解决。也就是：

```
define_callbacks :save, scope: [:kind, :name]
```

**Rails 里 Callback 相关的模块及继承关系**

```
ActiveRecord::Callbacks
           |
           V
ActiveModel::Callbacks
           |
           V
ActiveSupport::Callbacks
```

```
ActiveModel::Validations::Callbacks
ActiveJob::Callbacks
ActionDispatch::Callbacks
AbstractController::Callbacks
         |
         V
ActiveSupport::Callbacks
```

**Filters 顺序**

一条"回调链"上可以有多个"回调"，它们彼此之间不是独立的，有先后顺序。即：

- Before，After，Around

但这些"回调"也有终结的时候。即：

- End

**定义、运行回调**

`define_callbacks` 定义的时候会：

```
define_callbacks
       |
       V
set_callbacks name, CallbackChain.new(name, options)  # 这里的 name 表示"回调链"的名字。

def set_callbacks(name, callbacks) # 这里的 callbacks 是"回调链"的实例对象。
  send "_#{name}_callbacks=", callbacks
end

define_callbacks
       |
       V
def _run_#{name}_callbacks(&block)
  _run_callbacks(_#{name}_callbacks, &block)
end
```

`run_callbacks` 运行的时候会：

```
run_callbacks
      |
      V
_run_#{kind}_callbacks # 这里的 kind 表示"某种类型的"回调链
      |
      V
_run_callbacks(_#{name}_callbacks, &block) # 这里的 name 表示"回调链"
      |
      V
runner = callbacks.compile # 这里的 callbacks 表示"回调链"；
                           # compile 会创建 Callback 实例对象并做后续处理。
e = Filters::Environment.new(self, false, nil, block)
runner.call(e).value
```
