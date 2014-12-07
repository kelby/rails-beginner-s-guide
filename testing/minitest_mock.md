## Mock

类方法

```
expect
verify
```

`expect`
第一个参数为方法名，第二个参数为执行此方法后返回的结果，第三个参数表示传递给此方法的参数，也可以传递一个 block.

使用举例：

```ruby
Expect that method name is called, optionally with args or a blk, and returns retval.
m = Minitest::Mock.new
m.expect(:raiser, nil) do |args|
  raise RuntimeError, "this code path triggers an exception"
end
```

我们可以使用 Minitest::Mock.new 模拟一些不能(或不希望)直接调用的对象。

```ruby
user = Minitest::Mock.new
user.expect(:delete, true) # returns true, expects no args

UserDestoyer.new.delete_user(user)

assert user.verify
```

`verify`
mock 出来的对象有时候并不是我们想要的(计算机不够聪明)，verify 验证是否真的执行了上述方法。RSpec 在这点上使用起来更简洁，但也相差不大。

链接 [MiniTest::Mock](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/minitest/rdoc/MiniTest/Mock.html)
