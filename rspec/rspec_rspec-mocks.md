## rspec-mocks

```
double
instance_double
class_double
object_double

allow...to receive
allow...to receive...with
allow...to receive_messages
allow...to and_return

spy
instance_spy
class_spy
object_spy

expect...to have_received
expect...to_not have_received

allow...to receive...once
allow...to receive...twice
allow...to receive...thrice
allow...to receive...exactly..times
allow...to receive...at_least
allow...to receive...at_most

allow...to receive...ordered
allow...to receive...with...ordered

expect...to receive
expect...to receive...and_return
expect...to receive...exactly...and_return
expect...to receive...and_raise
expect...to receive...and_throw
expect...to receive...and_yield

expect...to receive...and_call_original

expect...to receive...with...and_raise
```

```
allow_any_instance_of...to receive...and_return
expect_any_instance_of...to receive...and_return
```

```
stub_const
hide_const
```

以下几个方法，文档没找到，在源代码里有：

```
stub
stub_chain

should_receive
should_not_receive

unstub
```

链接

[RSpec Mocks](http://www.rubydoc.info/github/rspec/rspec-mocks)
<br>
[RSpec Mocks 3.1](https://relishapp.com/rspec/rspec-mocks/docs)
