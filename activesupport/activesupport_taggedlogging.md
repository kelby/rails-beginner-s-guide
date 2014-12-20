## Tagged Logging

封装了 Ruby 标准的 [Logger](http://ruby-doc.org/stdlib-2.1.0/libdoc/logger/rdoc/index.html)，让我们能够给 logger 实例对象打'标签'。

```ruby
logger = ActiveSupport::TaggedLogging.new(Logger.new(STDOUT))

logger.tagged('BCX') { logger.info 'Stuff' }
# => Logs "[BCX] Stuff"

logger.tagged('BCX', "Jason") { logger.info 'Stuff' }
# Logs "[BCX] [Jason] Stuff"

logger.tagged('BCX') { logger.tagged('Jason') { logger.info 'Stuff' } }
# Logs "[BCX] [Jason] Stuff"
```

Rails.logger 默认已经使用。

实现：封装了 Logger 的实例对象，覆盖了它自带的 [Formatter](http://ruby-doc.org/stdlib-2.1.0/libdoc/logger/rdoc/Logger/Formatter.html) 对象，并添加了 tagged 方法。
