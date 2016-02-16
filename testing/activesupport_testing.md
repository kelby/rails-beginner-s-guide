## Active Support

包含了：Test Case、Assertions、Declarative、Isolation、Setup 和 Teardown 相关类方法、Time Helpers、Log Subscriber Test Helper 等部分。

#### Test Case

```ruby
assert_nothing_raised

# 为了向之前的 test/unit 兼容
alias :assert_raise :assert_raises
alias :assert_not_empty :refute_empty
alias :assert_not_equal :refute_equal
alias :assert_not_in_delta :refute_in_delta
alias :assert_not_in_epsilon :refute_in_epsilon
alias :assert_not_includes :refute_includes
alias :assert_not_instance_of :refute_instance_of
alias :assert_not_kind_of :refute_kind_of
alias :assert_no_match :refute_match
alias :assert_not_nil :refute_nil
alias :assert_not_operator :refute_operator
alias :assert_not_predicate :refute_predicate
alias :assert_not_respond_to :refute_respond_to
alias :assert_not_same :refute_same
```

```
test_order
test_order=
```

test_order 支持：

```ruby
ActiveSupport::TestCase.test_order # => :sorted

:random   # 随机
:parallel # 并行
:sorted   # 按字母顺序
:alpha    # 按字母顺序
```

#### Assertions

```
assert_difference
assert_no_difference

assert_not
```

#### Declarative

```
test
```

#### Isolation

类方法：

```
forking_env?
```

实例方法：

```
run
```

Forking：

```
run_in_isolation
```

Subprocess：

```
run_in_isolation
```

#### Setup & Teardown 相关类方法

```ruby
setup
teardown

# 文档没写，但还有：

before_setup
after_teardown
```

使用举例：

```ruby
class ExampleTest < ActiveSupport::TestCase
  setup do
    # ...
  end

  teardown do
    # ...
  end
end
```

#### Time Helpers

```
travel
travel_back
travel_to
```

#### Log Subscriber - Test Helper

```
set_logger

setup
teardown

wait
```

使用举例：

```ruby
class SyncLogSubscriberTest < ActiveSupport::TestCase
  include ActiveSupport::LogSubscriber::TestHelper

  def setup
    ActiveRecord::LogSubscriber.attach_to(:active_record)
  end

  def test_basic_query_logging
    Developer.all.to_a
    wait # <-- 这个
    assert_equal 1, @logger.logged(:debug).size
    assert_match(/Developer Load/, @logger.logged(:debug).last)
    assert_match(/SELECT \* FROM "developers"/, @logger.logged(:debug).last)
  end
end
```

#### File Fixtures

```
file_fixture
```

#### Constant Lookup

```ruby
determine_constant_from_test_name
```

#### Deprecation

```
assert_deprecated
assert_not_deprecated
```

