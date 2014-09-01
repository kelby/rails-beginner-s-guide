# ActiveJob

**不同的延迟任务，一样的 API.**

在 Delayed Job、Resque、Sidekiq 等延迟任务之间切换，还要改代码？
以后就不用了...

虽然这几个延迟任务 gem 使用上类似，但语法上多少有一点不同。
新的 Active Job 组件统一了接口，使用和切换都会变得更容易。

## QueueAdapter

默认使用的 queue_adapter 是 :inline，你可以根据需要自己设置 queue_adapter.

已经支持 Delayed Job、Resque、Sidekiq 等常用延迟任务 gem.

```ruby
# default queue adapter
ActiveJob::Base.queue_adapter = :inline

# Adapters currently supported: :backburner, :delayed_job,
# :qu, :que, :queue_classic, :resque, :sidekiq, :sneakers, :sucker_punch
```

## QueueName

默认使用的 queue_name 是 "default"

可以定制：

```ruby
 queue_as :my_jobs
 ```

## Enqueuing 入队与重试

常用方法：

```ruby
# Enqueue a job to be performed as soon the queueing system is free.
MyJob.enqueue record

# Enqueue a job to be performed tomorrow at noon.
MyJob.enqueue_at Date.tomorrow.noon, record

# Enqueue a job to be performed 1 week from now.
MyJob.enqueue_in 1.week, record
```

任务失败，还可以：

```ruby
retry_now
retry_in
retry_at
```

## Execution 执行

```ruby
# 不是 perform、run、work
execute(job_id, *serialized_args)
```

## Callbacks 回调

比某些延迟 gem 多做了一点点，除了队列&执行本身外，还可以有回调：

```ruby
before_enqueue
around_enqueue
after_enqueue

before_perform
around_perform
after_perform
```

## Logging

around_enqueue、around_perform 和 before_enqueue 有日志记录

enqueue、enqueue_at、perform_start、perform 等过程也有日志记录

## Identifier

每个任务都有全局唯一的 job_id

## Others

有利必有弊，可能面临以下问题：<br>
原 gem 本身的特点没能充分利用，灵活性降低，和其它 gem 的配合会变复杂。
