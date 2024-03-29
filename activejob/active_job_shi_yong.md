### Queue Adapter

定义：配置使用哪种后端任务、队列管理方式。默认使用的 queue\_adapter 是 `:inline`，处理方式是立即执行任务。你需要自己设置 queue\_adapter.

```ruby
ActiveJob::Base.queue_adapter = :inline
# 或类似
Rails.application.config.active_job.queue_adapter = :test
```

已经支持 Resque、Sidekiq、Delayed Job 等常用延迟任务 gem，所有可用 adapter:

```ruby
:backburner, :delayed_job, :qu, :que, :queue_classic,
:resque, :sidekiq, :sneakers, :sucker_punch,
:inline, :test
```

### Queue Name

定义：任务都是先进队列里，队列都有名字的，方便管理。默认使用的 queue\_name 是 "default"

可以定制：

```ruby
class MyJob < ActiveJob::Base
  queue_as :my_jobs

  def perform(record)
    # ...
  end
end
```

通过 `config.active_job.queue_name_prefix=` 可给所有队列名加前缀。

### Queue Priority

定义：队列有优先级这个属性，优先级高的会被先执行。类方法 `queue_with_priority` 可以进行设置，对整个类有效：

```ruby
class PublishToFeedJob < ActiveJob::Base
  queue_with_priority 50

  def perform(post)
    post.to_feed!
  end
end
```

可用实例方法 `priority` 获取，由上面统一设置的。

### Core

调用：设置任务所在队列、队列优先级行。类方法 `set` 使用举例：

```ruby
VideoJob.set(queue: :some_queue).perform_later(Video.last)
VideoJob.set(wait: 5.minutes).perform_later(Video.last)
VideoJob.set(wait_until: Time.now.tomorrow).perform_later(Video.last)

VideoJob.set(queue: :some_queue, wait: 5.minutes)
  .perform_later(Video.last)

VideoJob.set(queue: :some_queue, wait_until: Time.now.tomorrow)
  .perform_later(Video.last)

VideoJob.set(queue: :some_queue, wait: 5.minutes, priority: 10)
  .perform_later(Video.last)
```

`set` 支持可选参数：:wait、:wait\_until、:queue、:priority，它的具体实现由 ConfiguredJob 完成，主要是处理各个参数，起到配置作用。

> Note：可以不使用 `set` 直接调用 `perform_later` 进队列，然后等待执行。

类方法：

```
deserialize
```

实例方法：

```
serialize
deserialize
```

它们只是后端任务信息的一种方式，在此不必深究。

### Enqueuing 入队与重试

调用。

常用方法：

```
enqueue
```

使用举例：

```ruby
my_job_instance.enqueue

# 目前，只接受以下几种参数
my_job_instance.enqueue wait: 5.minutes
my_job_instance.enqueue queue: :important
my_job_instance.enqueue wait_until: Date.tomorrow.midnight
my_job_instance.enqueue priority: 10
# 若延迟 gem 本身不支持定时，会提示 wait、wait_until 不可用
```

入队列、执行任务失败，捕捉后，还可以重试：

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

### Execution 执行

调用。

```ruby
# 实例方法
perform_now
execute # 由子类，也就是我们所定义的 Job 实现具体内容

# 类方法
perform_now # 简单封装了实例方法 perform_now

execute
```

使用举例：

```ruby
MyJob.new(*args).perform_now

MyJob.perform_now("mike")
```

使用 `perform_now` 代码会立即执行，在这开发环境会很实用。

### Callbacks 回调

定义 + 自动调用。

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

> Note: 实现上，使用了 ActiveSupport::Callbacks 的 define\_callbacks、set\_callback、run\_callbacks 等方法。

### 提示

创建任务、进队列、执行任务这几个步骤，尽管我们可以区分开，但很多时候它们是交织在一起的\(从 API 上就能看出\)，我们可以不严格区分。

使用 Active Job 有利必有弊，可能面临以下问题：

原 gem 本身的特性没能充分发挥，灵活性降低，和其它 gem 的集成会变复杂。

