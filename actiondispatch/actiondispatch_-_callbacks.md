# ActionDispatch - Callbacks
```ruby
# 默认过滤类型
CALLBACK_FILTER_TYPES = [:before, :after, :around]

# 实例方法(必需调用它，回调才有用)
run_callbacks(kind, &block)

# 类方法
set_callback(name, *filter_list, &block)
skip_callback(name, *filter_list, &block)
reset_callbacks(name)

## 定义
define_callbacks(*names)

# 通过 name 产生化学反应：
"_#{name}_callbacks"
```

例子：
```ruby
class Report
  include ActiveSupport::Callbacks

  define_callbacks :print
  set_callback :print, :before, :before_print
  set_callback :print, :after, :after_print

  def print
    run_callbacks :print do
      p 'print'
    end
  end

  def before_print
    p 'before print'
  end

  def after_print
    p 'after print'
  end
end

Report.new.print
# => "before print"
# => "print"
# => "after print"
```

```ruby
define_model_callbacks :create, :update

# 同名方法
def create #:nodoc:
  run_callbacks(:create) { super }
end

# 同名方法
def update(*) #:nodoc:
  run_callbacks(:update) { super }
end

# 源代码
def define_model_callbacks(*callbacks)
  options = callbacks.extract_options!
  options = {
     :terminator => "result == false",
     :scope => [:kind, :name],
     :only => [:before, :around, :after]
  }.merge(options)

  types = Array.wrap(options.delete(:only))

  callbacks.each do |callback|
    define_callbacks(callback, options)

    types.each do |type|
      send("_define_#{type}_model_callback", self, callback)
    end
  end
end
```

为什么我们可以直接用 before_save, after_destroy 等方法，它们其实是语法糖，在 ActionModel::Callbacks 里有

`send("_define_#{type}_model_callback", self, callback)`

[Dig Deep Into Callbacks in Rails](http://ungsophy.github.io/blog/2012/05/28/dig-deep-into-callbacks-in-rails/)
