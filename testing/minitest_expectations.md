## ~~Expectations~~

可以用 assert_x 和 refute_x 来代替这里的方法，文档和实现都一样。

实例方法：

```
# 以下方法和 RSpec 对应，可以使用 assert_x 代替
must_be
must_be_close_to
must_be_empty
must_be_instance_of # 范围比 kind_of 小，子类的实例不行
must_be_kind_of     # 范围比 instance_of 大，子类的实例也行
must_be_nil
must_be_same_as
must_be_silent
must_be_within_delta
must_be_within_epsilon
must_equal
must_include
must_match
must_output
must_raise
must_respond_to
must_send
must_throw

# must_x 和 wont_x 意思正好反着来

# 以下方法和 RSpec 对应，可以使用 refute_x 代替
wont_be
wont_be_close_to
wont_be_empty
wont_be_instance_of
wont_be_kind_of
wont_be_nil
wont_be_same_as
wont_be_within_delta
wont_be_within_epsilon
wont_equal
wont_include
wont_match
wont_respond_to
```

链接 [MiniTest::Expectations](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/minitest/rdoc/MiniTest/Expectations.html)
