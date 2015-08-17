## Active Job

#### Test Helper

实例方法：

```
assert_enqueued_with
assert_performed_with

assert_enqueued_jobs
assert_no_enqueued_jobs

assert_performed_jobs
assert_no_performed_jobs
```

```
perform_enqueued_jobs
```

以及：

```ruby
delegate :enqueued_jobs, :enqueued_jobs=, :performed_jobs, :performed_jobs=,
         to: :queue_adapter
```

默认测试环境下使用 queue_adapter 是 test，类似：

```ruby
Rails.application.config.active_job.queue_adapter = :test
```
