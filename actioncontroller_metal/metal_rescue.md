#### ~~Rescue~~

执行到具体 action 抛异常时会检测是否需要抛异常，如果是的话，抛异常。

有 `process_action` 同名方法。

`show_detailed_exceptions?` (默认是 false，但本地请求的话是 true)，可配置 config.consider_all_requests_local

`rescue_with_handler`(封装了 ActiveSupport::Rescuable 的同名方法)
