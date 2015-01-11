#### alias_method_chain

不修改原有接口和调用代码，给接口添加额外的行为。

举例：

```ruby
# 代码一
1  class Klass
2    def salute
3      puts "Aloha!"
4      # do some other things...
5    end
6  end
7
8  Klass.new.salute
9  # => Aloha!
10 # do some other things...
```

张三编写了以上代码，并提供接口给大家使用。李四用着不爽，因为没有日志记录。他想到了一个方法：

```ruby
puts "Before do salute ..."
Klass.new.salute # => Aloha!
puts "End do salute ..."
```

李四觉得这么做非常好，于是大力推荐别人也这么做，其中就包括王五。但王五觉得李四疯了，因为这意味着要修改已有代码，并且很丑陋。

李四想了想，于是这么做...

```ruby
# 代码二
1  class Klass
2    def salute_with_log
3      puts "Before do salute ..."
4      salute_without_log
5      puts "... End do salute"
6    end
7
8    alias_method :salute_without_log, :salute
9    alias_method :salute, :salute_with_log
10 end
```

它利用"猴子补丁"并且两次使用 `alias_method` 以达到效果，检验一下：

```ruby
Klass.new.salute     # -> 调用代码一，第 2 行；
                     # 但代码二，第 9 行更改了调用；
                     # 实际运行的是代码二，第 2 行。

# 输出：
Before do salute ... # -> 对应代码二，第 3 行

# 接下来运行代码二，第 4 行；但代码二，第 8 行更改了调用；实际运行的是代码一，第 2 行

# 输出
Aloha!

# 接下来运行代码二，第 5 行

# 输出
... End do salute
```

完美，既没有修改张三的接口，也没有修改王五的调用代码。  
只要张三提供的接口不变，王五的调用代码就不用修改，而自己的代码也能顺利运行。

但上面隐藏着一个很大的问题：代码二，第 8、9 行顺序不能颠倒，一不小心的话，将进入死循环里去...

```
Calling method...
Calling method...
Calling method...
SystemStackError: stack level too deep
```

使用 `alias_method_chain` 可以防患于未然，不必再担忧：

```ruby
class Klass
  def salute
    puts "Aloha!"
  end
end

Klass.new.salute
# => Aloha!

class Klass
  def salute_with_log
    puts "Begin do salute ..."
    salute_without_log
    puts "... End do salute"
  end
  alias_method_chain :salute, :log
end

Klass.new.salute
# =>
# Begin do salute ...
# Aloha!
# ... End do salute
```
