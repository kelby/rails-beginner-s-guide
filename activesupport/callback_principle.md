## Callback 原理

殊途同归，Rails 在几个地方提供的几个回调方法最终都来到 ActiveSupport::Callbacks

而 ActiveSupport::Callbacks 本身又分为几部分。

**类方法**

这是对外提供的接口，其它几个模块里的 Callbacks 方法都是直接使用或封装后而来的。

有方法：

```
set_callback
skip_callback
reset_callbacks
define_callbacks

# 以及 protected 方法
get_callbacks
set_callbacks
```

一般，用 define_callbacks & set_callback 定义回调。

**实例方法**

```
run_callbacks
```

一般，用 run_callbacks 运行回调。

**Callback**

任何方式或参数定义的回调，最终都会对应着一个 Callback 实例对象。

实例对象包含信息

- 回调类型(type): 

对于 Rails 而言，只能是 :before, :after, :around 之一。(见 CALLBACK_FILTER_TYPES)

- 过滤代码(filter): 

真正要执行的过滤代码。

过滤代码支持类开:

```
Symbols:: 方法名。
Strings:: 可求值的字符串。
Procs::   proc 对象。
Objects:: 普通的对象，但必需有类似 before_x, around_x, after_x 等同名回调方法。
          因为，之后会以拼接字符串的形式，找出对应的回调方法，然后 send 调用。

其实，还有一种类型 Conditionals 但被直接跳过了，所以不算在内。
```

这些不同类型的代码都会被转换成 lambda 对象，之后一视同仁，处理过程一致。

- 属于哪条回调链(chain): 

比如 save链、validation链、destroy链等，可用 a_record._x_callbacks 查看。define_callbacks 时决定。

- 可选参数：

这个不用解释了吧。

根据以上解释，可生成一条 callback，并加入相应回调链里。

**CallbackChain**

上面提到的回调链，每一条链都是 CallbackChain 的实例对象。

Rails 没有提供查看所有 callback chain 信息的接口，只能通过 hack 方式查看。

比如：

```
ObjectSpace.each_object(ActiveSupport::Callbacks::CallbackChain).select do |cc|
  cc.name.present?
end

或

ClassName.methods.select{|c| c.to_s =~ /^_[a-z]+_callbacks$/ }
```

**Filters**

一条链上可以有多段过滤代码，它们彼此之间不是独立的，有先后顺序等关系。

- Before，After，Around

过滤代码的先后顺序关系，有 3 种情况，它们分别对应以上类。

- End

过滤方法是一个接一个的，所以有上面的 Before, After, Around。但也有终结的时候，此时用 End 表示。

---

**关键代码之一**

运行的时候会：

```ruby
runner = callbacks.compile
e = Filters::Environment.new(self, false, nil, block)
runner.call(e).value
```

上面 callbacks 属于 CallbackChain 的实例对象。它有 compile 方法。

**其它封装后引出的概念**

```
define_callbacks :save
set_callback :save, :before, Audit.new

define_callbacks :save, scope: [:name] # 用 as 表示，是否更形象？
```

kind 是 before

name 是 save

**传递给回调的是实例对象，怎么工作？**

```
filter.public_send method_to_call, target, &blk
```

filter 就是实例对象。

method_to_call 根据回调链(chain_config)，拼接而来，也就是回调名字。也就是实例对象将要执行的方法。

target 也就是调用回调时会影响到的对象。在这里做为参数供实例对象执行。
