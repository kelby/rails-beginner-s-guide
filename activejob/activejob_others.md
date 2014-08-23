# Others

## Arguments 参数处理

接受的参数类型很广泛，需要先处理一下。

进队列时参数需要 serialize
执行前参数需要 deserialize

当然，这都是自动完成的。

## Railtie

设置 logger 和默认 queue_adapter

## QueueAdapters

原来，不同的延迟任务 gem，现在：

都有同名的 self.enqueue 和 self.enqueue_at

有各自不同的 self.perform、perform、run、work
