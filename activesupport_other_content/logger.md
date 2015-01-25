## Logger Silence

提供 Logger 的实例方法：

```
silence
```

使用举例：

```ruby
Rails.logger.silence do
  # ... 这里面，"程序所输出的日志"会被清除掉。
end
```

可以通过以下设置，不用此特性：

```ruby
ActiveSupport::Logger.silencer = false
```

此时，silence 所包含的 block 里的"程序所输出的日志"不会被清除。

链接

[标准库 Logger](http://ruby-doc.org/stdlib-2.2.0/libdoc/logger/rdoc/index.html)
