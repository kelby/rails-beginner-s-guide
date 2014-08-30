# Misc

它在哪被调用，和它类似或相关的模块之间是什么关系，有何不同？

## 心得：

- C < B < A 有时候之所以 B 要 extend A 并不是为了 B 自己使用，而仅仅是为了方便 C

---

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