## ActiveSupport

### Test Case

```
assert_nothing_raised

test_order
test_order=
```

### Assertions

```
assert_difference
assert_no_difference

assert_not
```

### Declarative

```
test
```

### Isolation

```
forking_env?

run
```

**Forking**

```
run_in_isolation
```

**Subprocess**

```
run_in_isolation
```

### Setup And Teardown ClassMethods

```
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

### Time Helpers

```
travel
travel_back
travel_to
```

### LogSubscriber Test Helper

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
