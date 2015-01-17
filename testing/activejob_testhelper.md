## Active Job

### Test Helper

实例方法：

```
assert_enqueued_with
assert_performed_with

assert_enqueued_jobs
assert_no_enqueued_jobs

assert_performed_jobs
assert_no_performed_jobs

before_setup
after_teardown

clear_enqueued_jobs
clear_performed_jobs

perform_enqueued_jobs

queue_adapter
```

以及：

```ruby
delegate :enqueued_jobs, :enqueued_jobs=, :performed_jobs, :performed_jobs=,
         to: :queue_adapter
```

除上述外方法外，测试环境下可以使用 Test Adapter 适配器。

```ruby
Rails.application.config.active_job.queue_adapter = :test
```
