## DeliveryJob

Action Mailer 默认就已经支持异步发送，因为它使用了 Rails 自带的 ActiveJob.

具体实现体现在这个 class 上，它完成了指定队列名，及 perform 等工作。
