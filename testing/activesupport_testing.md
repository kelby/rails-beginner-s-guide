# ActiveSupport Testing

## TestCase

```
assert_nothing_raised
test_order, test_order=
```

## Assertions

```
assert_difference
assert_no_difference
assert_not
```

## SetupAndTeardown

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
