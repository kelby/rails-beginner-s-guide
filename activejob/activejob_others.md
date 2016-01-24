其它多个类或模块，统一在此列举。

### Async Job

已经异步了，再给你一个选择：是否要并发？

```ruby
Rails.application.config.active_job.queue_adapter = :async
```

进队列后，任务运用 Ruby 自身的并发。数据放在内存里，运行速度加快了；没有持久化，重启会丢失数据。

一般来说，生产环境你不能使用，但开发与测试或许可以体验。

### Arguments 参数处理

接受的参数类型很广泛，需要先处理一下。

进队列时参数需要 `serialize`，
执行前参数需要 `deserialize`

当然，这都是自动完成的。

### 参数支持 Global ID

一般入队列(enqueue_in、enqueue_at 和 enqueue) 只传能够标识对象的那部分参数(如：class、id)，出队列/执行的时候再根据这些参数获取对象。

但因为 serialize_argument 支持的类型有多种，其中就包括 GlobalID::Identification. 所以我们可以传递一个"活的对象"进队列，而不只是它的一部分(如：class、id).

使用 Global ID 前后对比。

之前：

```ruby

class TrashableCleanupJob
  def perform(trashable_class, trashable_id, depth)
    # 出队列/执行的时候需要根据 trashable_class 和 trashable_id 查询相应 trashable
    trashable = trashable_class.constantize.find(trashable_id)
    trashable.cleanup(depth)
  end
end
```

之后：

```ruby
class TrashableCleanupJob
  def perform(trashable, depth)
    # 出队列/执行的时候直接使用 trashable
    trashable.cleanup(depth)
  end
end
```

> Note: 不规范的写法里，也可以直接传递对象。

### Queue Adapters

原来，不同的延迟任务 gem 有各自不同的 self.perform、perform、run、work，现在：

都有同名的 self.enqueue 和 self.enqueue_at

### Logging

around_enqueue、around_perform 和 before_enqueue 有日志记录

enqueue、enqueue_at、perform_start、perform 等过程也有日志记录

### Identifier

每个任务都有全局唯一的 job_id

### Configured Job

配置实例，Core 里的 set 类方法会用到它。可实例化 Job 类，并调用 perform_now 或 perform_later

### 解析 queue_adapter 及其 API

queue_adapter 是 Delayed Job、Resque、Sidekiq 等不同的延迟任务抽象而来。

而 queue_adapter 所用的 API(enqueue_at、enqueue_in、enqueue 等)，也是从原延迟任务所提供的 API 抽象而来。

### 命令行快捷生成

```bash
rails generate job NAME [options]
```

### 异常捕获与处理

使用 Active Support 的异常捕获方法 `rescue_from`

```ruby
class GuestsCleanupJob < ActiveJob::Base
  queue_as :default
 
  # 异常捕获
  rescue_from(ActiveRecord::RecordNotFound) do |exception|
   # 异常处理
  end
 
  def perform
    # ...
  end
end
```
