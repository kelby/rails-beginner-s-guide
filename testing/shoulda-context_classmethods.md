## ClassMethods

```
context

should
should_not
should_eventually
before_should

subject
subject_block
described_type
```

链接 [ClassMethods](http://www.rubydoc.info/github/thoughtbot/shoulda-context/Shoulda/Context/ClassMethods)

`should` 方法和 MiniTest 里的 it 方法相对应，本质都是定义一个 test 方法。

`context` 方法应该有共同的 setup/teardown 方法，并且它可以嵌套使用，彼此之间不影响。

`subject` 方法和 MiniTest 里一样，在此不再介绍。

和 Context 一样，ClassMethods 源代码在 context.rb 里面。另外，对应的还有 InstanceMethods，因为不常用，在此不再介绍。
