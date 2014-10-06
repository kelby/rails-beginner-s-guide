# LogSubscriber

```ruby
module ActiveRecord
  class LogSubscriber < ActiveSupport::LogSubscriber
    def sql(event)
      "#{event.payload[:name]} (#{event.duration}) #{event.payload[:sql]}"
    end
  end
end
```

```ruby
ActiveRecord::LogSubscriber.attach_to :active_record
```

## TestHelper

```ruby
class SyncLogSubscriberTest < ActiveSupport::TestCase
  include ActiveSupport::LogSubscriber::TestHelper

  def setup
    ActiveRecord::LogSubscriber.attach_to(:active_record)
  end

  def test_basic_query_logging
    Developer.all.to_a
    wait
    assert_equal 1, @logger.logged(:debug).size
    assert_match(/Developer Load/, @logger.logged(:debug).last)
    assert_match(/SELECT \* FROM "developers"/, @logger.logged(:debug).last)
  end
end
```
