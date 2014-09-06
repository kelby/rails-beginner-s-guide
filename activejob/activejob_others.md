# Others

## Arguments 参数处理

接受的参数类型很广泛，需要先处理一下。

进队列时参数需要 serialize
执行前参数需要 deserialize

当然，这都是自动完成的。

## GlobalID support for Arguments

一般入队列(enqueue_in、enqueue_at 和 enqueue) 只传能够标识对象的那部分参数(如：class、id)，出队列/执行的时候再根据这些参数获取对象。

但因为 serialize_argument 支持的类型有多种，其中就包括 GlobalID::Identification. 所以我们可以传递一个"活的对象"进队列，而不只是它的一部分(如：class、id).

使用 GlobalID 前后对比：

```ruby
class TrashableCleanupJob
  def perform(trashable_class, trashable_id, depth)
    # 出队列/执行的时候需要根据 trashable_id 获取 trashable
    trashable = trashable_class.constantize.find(trashable_id)
    trashable.cleanup(depth)
  end
end

class TrashableCleanupJob
  def perform(trashable, depth)
    # 出队列/执行的时候直接使用 trashable
    trashable.cleanup(depth)
  end
end
```

> Note: 不规范的写法里，也可以直接传递对象。

## Railtie

设置 logger 和默认 queue_adapter

## QueueAdapters

原来，不同的延迟任务 gem，现在：

都有同名的 self.enqueue 和 self.enqueue_at

有各自不同的 self.perform、perform、run、work

## 解析 queue_adapter 及其 API

queue_adapter 是 Delayed Job、Resque、Sidekiq 等不同的延迟任务抽象而来。

而 queue_adapter 所用的 API(enqueue_at、enqueue_in、enqueue 等)，也是从原延迟任务所提供的 API 抽象而来。
