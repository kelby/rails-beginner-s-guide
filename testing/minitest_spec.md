## Spec DSL

实例方法

```
it

let
subject

before
after
```

`it` 和 def test_x 类似。一般的，每一个测试案例对应着一个测试方法。可用 it 定义它们

`let` 和 def 定义方法类似，只不过这里方法内容始终是 block. 有延迟加载的作用

`subject` 和 let 类似，但它没有名字。(其实它是有名字的，始终是 :subject)

`before` 前置过滤器，在每一个 it 之前执行。(名字无所谓，只是起标识作用而矣)

`after` 后置过滤器，在每一个 it 之后执行。(名字无所谓，只是起标识作用而矣)

> before、after 方法和 RSpec 对应，可以使用 setup、teardown 代替

除上述方法外，还有

```
spec_type
register_spec_type

children
```

链接 [MiniTest::Spec::DSL](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/minitest/spec/rdoc/MiniTest/Spec/DSL.html)
