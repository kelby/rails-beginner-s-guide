## Notifications

怎么使用？

- subscribe  - 订阅
- instrument - 发布

```ruby
# 发布消息
ActiveSupport::Notifications.instrument('render', extra: :information) do
  # 下面是真正要执行的内容
  render text: 'Foo'
end

# 订阅消息
ActiveSupport::Notifications.subscribe('render') do |name, start, finish, id, payload|
  # 以下 4 个属性是 notification 自带的
  name    # => 类型是字符串, 代表 notification 的名字(在这里是 'render')
  start   # => 类型是 Time, 代表上面开始"执行内容"的时间
  finish  # => 类型是 Time, 代表上面结束"执行内容"的时间
  id      # => 类型是 String, 唯一的 ID 表示此 notification

  # 以下属性对应着 instrument 里的 extra
  payload # => 类型是 Hash, 也就是上面传递过来的参数
end
```

> Note: 可以按正则规则进行"订阅"。

为什么使用？

Rails 内外，都有很多类似、可替换的技术。但这个方法，即能保证在同一项目内进行，又能使耦合度尽可能的降低。并且，它使用范围很广。

缺点：配置不方便，一启动就执行，并且更改要重启。默认对性能是有影响的，subscribe 并不是异步执行。

Rails 默认有很多 Instrumentation，你可以不写 instrument 直接 subscribe 它们。或者，你自己写 instrument，然后 subscribe.

有以下特点：

- 低耦合，并且轻量化
- 操作简单
- 随处可用

实现：运用中间变量 notifier.

> Note: 注意使用场景。这里并不是严格的发布者、订阅者模型，你从它们的方法名及默认参数就应该知道。

在控制台里，可运行以下命令，查看 Rails 项目里有哪些 instrumenter：

```ruby
ActiveSupport::Notifications.instrumenter.class
# => ActiveSupport::Notifications::Instrumenter

fanout = ActiveSupport::Notifications.instrumenter.instance_variable_get("@notifier")

fanout.class
# => ActiveSupport::Notifications::Fanout

fanout.instance_variable_get("@subscribers").map do |s|
  s.instance_variable_get "@pattern"
end
```

> Note: 此命令包含的 instrumenter 并不完整。比如：元编程创建的 instrumenter 就不包含。(Action Controller 有很多 instrumenter 都是元编程生成的)

所有方法:

```
instrument
instrumenter

publish

subscribe
subscribed
unsubscribe
```
