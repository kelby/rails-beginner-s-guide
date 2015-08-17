### Action Mailer Log Subscriber

日志记录，继承于 ActiveSupport::LogSubscriber，执行哪个方法时想要记录日志，只需要创建和它同名方法，然后打印日志即可。Log Subscriber 章节会讲到。

目前 Rails 侦听以下方法：

```
deliver - 发送
receive - 接收
process - 处理
```

除上述方法外，还有：

```
logger
```
