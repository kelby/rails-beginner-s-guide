#### ~~Instrumentation~~

当执行以下同名方法时，会发送消息，及有时间记录(以观察性能等指标)。

```
process_action

redirect_to

render

send_data
send_file
```

用到了 `ActiveSupport::Notifications.instrument` 和 `Benchmark.ms`
