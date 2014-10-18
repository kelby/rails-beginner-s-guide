# ActiveSupport

## Test Case

```
assert_nothing_raised

test_order
test_order=
```

## Assertions

```
assert_difference
assert_no_difference

assert_not
```

## Setup And Teardown

```
setup
teardown
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

## TimeHelpers

```
travel, travel_back, travel_to
```

## LogSubscriber Test Helper

```
set_logger, setup
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
