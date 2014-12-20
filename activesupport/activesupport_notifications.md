## Notifications

怎么使用？

- subscribe  - 订阅
- instrument - 发布

```ruby
# 发布消息
ActiveSupport::Notifications.instrument "my.custom.event", this: :data do
  # ... 要执行的内容
end

# 订阅消息
ActiveSupport::Notifications.subscribe "my.custom.event" do |name, started, finished, unique_id, data|
  puts data.inspect # {:this=>:data}
end
```

为什么使用？

Rails 内外，都有很多类似、可替换的技术。但这个方法，即能保证在同一项目内进行，又能使耦合度尽可能的降低。并且，它使用范围很广。

缺点：配置不方便，一启动就执行，并且更改要重启。默认对性能是有影响的，subscribe 并不是异步执行。

Rails 默认有很多 Instrumentation，你可以不写 instrument 直接 subscribe 它们。或者，你自己写 instrument，然后 subscribe。

控制台里运行 `ActiveSupport::Notifications.instrumenter` 可以查看有哪些 Instrumentation

有以下特点：

- 低耦合，并且轻量化
- 操作简单
- 随处可用

实现：运用中间变量 notifier

> Note: 注意使用场景。这里并不是严格的发布者、订阅者模型，你从它们的方法名及默认参数就应该知道。

> Note: 直接查看 Rails 项目里有哪些 instrumenter 可运行命令
`ActiveSupport::Notifications.instrumenter.instance_variable_get("@notifier").instance_variable_get("@subscribers").map { |s| s.instance_variable_get "@pattern" }`
此命令不包含元编程创建的 instrumenter，如 ActionController 就有很多 instrumenter 没有包含在内。

所有方法:

```
instrument
instrumenter

publish

subscribe
subscribed
unsubscribe
```

参考

[Pssst... your Rails application has a secret to tell you](http://signalvnoise.com/posts/3091-pssst-your-rails-application-has-a-secret-to-tell-you)<br>
[Digging Deep with ActiveSupport::Notifications](https://speakerdeck.com/nextmat/digging-deep-with-activesupportnotifications)<br>
[#249 Notifications in Rails 3](http://railscasts.com/episodes/249-notifications-in-rails-3)<br>
[ActiveSupport::Notifications, statistics and using facts to improve your site](http://www.reinteractive.net/posts/141-activesupport-notifications-statistics-and-using-facts-to-improve-your-site)<br>
[Active Support Instrumentation](http://edgeguides.rubyonrails.org/active_support_instrumentation.html)

