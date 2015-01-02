# Active Support autoload 的类和模块

- Concern

- Dependencies

包括：Class Cache 和 Watch Stack

- ~~Descendants Tracker~~

- File Update Checker

- Notifications

- Subscriber

- Log Subscriber

- Rescuable

- Safe Buffer

- Test Case

---

```
execute_hook

on_load

run_load_hooks
```

使用举例：

```ruby
initializer 'active_record.initialize_timezone' do
  ActiveSupport.on_load(:active_record) do
    self.time_zone_aware_attributes = true
    self.default_timezone = :utc
  end
end
```

```ruby
ActiveSupport.run_load_hooks(:active_record, ActiveRecord::Base)
```
