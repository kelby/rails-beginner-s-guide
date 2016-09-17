## DeliveryJob

Action Mailer 默认就已经支持异步发送，因为它使用了 Rails 自带的 ActiveJob.

具体实现体现在这个 class 上，它完成了使用 `queue_as` 指定队列名，使用 `rescue_from` 异常报错，及 perform 等工作（尽管 perform 还是调用具体 Mailer 里的代码）。
