## Callbacks 回调的源头

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

| 方法 | 解释 |
| -- | -- |
| define_callbacks | 声明有一个回调 |
| set_callback | 声明与这个回调有关的回调操作，以及回调操作的先后顺序 |
| run_callbacks | 第二个参数是个 block，是真正要执行的操作，在执行这个 block 的前后会执行回调操作。执行什么回调？由第一个参数决定，并且这里的回调必须事前由 define_callbacks 声明过。执行这些回调操作的顺序？必须事前由 set_callback 声明过。 |

例子：

```ruby
1  class Report
2    include ActiveSupport::Callbacks
3
4    define_callbacks :print
5    set_callback :print, :before, :before_print
6    set_callback :print, :after, :after_print
7
8    def print
9      run_callbacks :print do
10        p 'print'
11      end
12     # 所有真正要执行的操作，以 block 的形式，当做 run_callbacks 的第二个参数。
13    end
14
15    def before_print
16      p 'before print'
17    end
18
19    def after_print
20      p 'after print'
21    end
22  end

Report.new.print
# => "before print"
# => "print"
# => "after print"
```

第 4 行，声明有一个回调。这里的名字和第 8 行没有关系，只是恰好相同。

第 5 行，
声明与这个回调(print)有关的回调操作(before_print)，以及回调作的先后顺序(before). 这里的回调操作以 before_ 为前缀，这不是要求，只是这里恰好这样。

第 6 行，
声明与这个回调(print)有关的回调操作(after_print)，以及回调操作的先后顺序(after). 这里的回调操作以 after_ 为前缀，这不是要求，只是这里恰好这样。

第 8 行，声明一个方法(操作)... 这个方法所有真正要执行的操作，以 block 的形式，当做 run_callbacks 的第二个参数。

第 9 行，
第二个参数是个 block，是真正要执行的操作(对应第 10 行)，在执行这个 block 的前后会执行回调操作。执行什么回调？由第一个参数决定，并且这里的回调必须事前由 define_callbacks 声明过。执行这个回调的顺序？必须事前由 set_callback 声明过。

## 其它

- Rails 里的语法糖

  - ActiveRecord::Callbacks
  - ActiveModel::Callbacks#define_model_callbacks
  - ActiveSupport::Callbacks.define_callbacks

- 查看有什么回调

`spu._save_callbacks`

这里的 save 由 define_callbacks 声明，是回调，不是操作。

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

参考

[Dig Deep Into Callbacks in Rails](http://ungsophy.github.io/blog/2012/05/28/dig-deep-into-callbacks-in-rails/)
