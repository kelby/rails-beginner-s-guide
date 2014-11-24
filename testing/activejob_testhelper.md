## ActiveJob

### Test Helper

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

queue_adapter
```

以及

```ruby
delegate :enqueued_jobs, :enqueued_jobs=, :performed_jobs, :performed_jobs=, to: :queue_adapter
```

### Test Adapter

```
enqueued_jobs

performed_jobs
```
