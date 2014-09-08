# Misc

## 心得：

- C < B < A 有时候之所以 B 要 extend A 并不是为了 B 自己使用，而仅仅是为了方便 C

## 继承关系及 ancestors

父类和模块都有此方法，子类没有(重写)此方法，执行顺序：模块按被加载的顺序逆序，最后到父类。

```ruby
module A
  def do_something
    puts 'A->do_something'
    super
  end
end

module B
  def do_something
    puts 'B->do_something'
    super
  end
end

module C
  def do_something
    puts 'C->do_something'
    super
  end
end


class Parent
  def do_something
    puts 'Parent->do_something'
  end
end

class Child < Parent
  include A
  include B
  include C
end

p Child.ancestors
# => [Child, C, B, A, Parent, Object, Kernel, BasicObject]


Child.new.do_something
# Output =>
#
# C->do_something
# B->do_something
# A->do_something
# Parent->do_something
```

> Note: 以上参考 [modules.rb](https://gist.github.com/andrewberls/8090332)

## inherited

`inherited(subclass)`

当前类定义子类时，就会触发此回调。
(类比 Module.html#included)

Example:

```ruby
class Foo
  def self.inherited(subclass)
    puts "New subclass: #{subclass}"
  end
end

class Bar < Foo
end

class Baz < Bar
end
```

produces:

```
New subclass: Bar
New subclass: Baz
```

另：继承于一个类，父类的类方法就是子类的类方法，父类的实例方法就是子类的实例方法。
