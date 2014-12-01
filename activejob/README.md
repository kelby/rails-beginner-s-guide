# Active Job

**不同的延迟任务，一样的 API.**

在 Delayed Job、Resque、Sidekiq 等延迟任务之间切换，还要改代码？
以后就不必了...

虽然这几个延迟任务 gem 使用上类似，但语法上多少有一点不同。
新的 ActiveJob 组件统一了接口，使用和切换都会变得更容易。

## Queue Adapter

默认使用的 queue_adapter 是 :inline，你可以根据需要自己设置 queue_adapter.

已经支持 Delayed Job、Resque、Sidekiq 等常用延迟任务 gem.

```ruby
# 默认 queue adapter
ActiveJob::Base.queue_adapter = :inline
# 或
Rails.application.config.active_job.queue_adapter = :test

# 所有可用 adapter: :backburner, :delayed_job, :qu, :que, 
# :queue_classic, :resque, :sidekiq, :sneakers, :sucker_punch, :inline, :test
```

## Queue Name

默认使用的 queue_name 是 "default"

可以定制：

```ruby
class MyJob < ActiveJob::Base
  queue_as :my_jobs

  # ...
end
```

通过 `config.active_job.queue_name_prefix=` 可给所有队列名加前缀。

## Core

```
# 实例方法
serialize

# 类方法
set # 常用
deserialize
```

使用举例：

```ruby
# Enqueue a job to be performed as soon the queueing system is free.
MyJob.perform_later record

# Enqueue a job to be performed tomorrow at noon.
MyJob.set(wait_until: Date.tomorrow.noon).perform_later(record)

# Enqueue a job to be performed 1 week from now.
MyJob.set(wait: 1.week).perform_later(record)
```

## Enqueuing 入队与重试

常用方法：

```
enqueue
```

使用举例：

```ruby
my_job_instance.enqueue

# 目前，只接受以下 3 种参数
my_job_instance.enqueue wait: 5.minutes
my_job_instance.enqueue queue: :important
my_job_instance.enqueue wait_until: Date.tomorrow.midnight
```

执行任务失败，还可以：

```ruby
retry_job
```

使用举例：

```ruby
class SiteScrapperJob < ActiveJob::Base
  rescue_from(ErrorLoadingSite) do
    retry_job queue: :low_priority
  end

  def perform(*args)
    # raise ErrorLoadingSite if cannot scrape
  end
end
```

除上述两实例方法外，还有类方法：

```
perform_later
```

## Execution 执行

```ruby
# 实例方法
perform, perform_now

# 类方法
perform_now # 简单封装了实例方法 perform_now
```

使用举例：

```ruby
MyJob.new(*args).perform_now

MyJob.perform_now("mike")
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

使用举例：

```ruby
class VideoProcessJob < ActiveJob::Base
  queue_as :default

  after_perform do |job|
    UserMailer.notify_video_processed(job.arguments.first)
  end

  def perform(video_id)
    Video.find(video_id).process
  end
end
```

其它几个方法类似。

实现上，都是直接封装 `set_callback`

## Others

有利必有弊，可能面临以下问题：  
原 gem 本身的特点没能充分利用，灵活性降低，和其它 gem 的集成会变复杂。
