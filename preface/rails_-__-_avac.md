Controller#actions 里定义实例变量，并通过 render 方法进行渲染。

ActionView 渲染过程很复杂：渲染器、上下文、渲染。

原理是否真的和以下类似。

---

## 标准库 ERB 举例

```ruby
require "erb"

# build data class
class Listings
  PRODUCT = { :name => "Chicken Fried Steak",
              :desc => "A well messages pattie, breaded and fried.",
              :cost => 9.95 }

  attr_reader :product, :price

  def initialize( product = "", price = "" )
    @product = product
    @price = price
  end

  def build
    b = binding
    # create and run templates, filling member data variables
    ERB.new(<<-'END_PRODUCT'.gsub(/^\s+/, ""), 0, "", "@product").result b
      <%= PRODUCT[:name] %>
      <%= PRODUCT[:desc] %>
    END_PRODUCT
    ERB.new(<<-'END_PRICE'.gsub(/^\s+/, ""), 0, "", "@price").result b
      <%= PRODUCT[:name] %> -- <%= PRODUCT[:cost] %>
      <%= PRODUCT[:desc] %>
    END_PRICE
  end
end

# setup template data
listings = Listings.new
listings.build

puts listings.product + "\n" + listings.price
```

输出结果

```
Chicken Fried Steak
A well messages pattie, breaded and fried.

Chicken Fried Steak -- 9.95
A well messages pattie, breaded and fried.
```

上面的例子，也许没能体现环境的变化，再来一个：

```ruby
# Setting instance var
@user = User.new( "Kelby" )

# Get binding
binder = self.send( :binding ) # calling a private method

# A simple template string
template = "Hello, <%= @user.name %>"

# Rendering template
ERB.new( template ).result( binder )

# Result
=> "Helo, Kelby"
```

> **Note:** 以上利用了中间变量 binder. binder 应为更高一级的变量(可以跨越 Controller#actions 和 View 两个环境)，Rails 也是如此设计的吗？

## Ruby 内建 Binding

Objects of class `Binding` encapsulate the execution context at some particular place in the code and retain this context for future use. The variables, methods, value of `self`, and possibly an iterator block that can be accessed in this context are all retained. Binding objects can be created using `Kernel#binding`, and are made available to the callback of     `Kernel#set_trace_func`.

These binding objects can be passed as the second argument of the `Kernel#eval` method, establishing an environment for the evaluation.

```ruby
class Demo
  def initialize(n)
    @secret = n
  end
  def get_binding
    return binding()
  end
end

k1 = Demo.new(99)
b1 = k1.get_binding
k2 = Demo.new(-3)
b2 = k2.get_binding

eval("@secret", b1)   #=> 99
eval("@secret", b2)   #=> -3
eval("@secret")       #=> nil
```

Binding objects have no class-specific methods.

## ERB Public Class Methods
new(str, safe_level=nil, trim_mode=nil, eoutvar='_erbout')

Constructs a new ERB object with the template specified in str.

## ERB Public Instance Methods

result(b=new_toplevel)

Executes the generated ERB code to produce a completed template, returning the results of that code. (See ::new for details on how this process can be affected by safe_level.)

b accepts a Binding or Proc object which is used to set the context of code evaluation.

## Erubis supports Ruby on Rails

Rails 用的是 gem 'erubis'，不是 Ruby 标准库里的 ERB，不过它们原理类似，不再深入。

## 这里相关

[Binding](http://ruby-doc.org/core-2.1.2/Binding.html)<br>
[ERB](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/erb/rdoc/index.html)<br>
[Erubis](https://github.com/genki/erubis)

