## Notifications

怎么使用？

- subscribe - ask to receive instrument data
- instrument - signal event or track data about operation(duration/exceptions)

```ruby
# 发布
ActiveSupport::Notifications.instrument "my.custom.event", this: :data do
  # do your custom stuff here
end

# 订阅
ActiveSupport::Notifications.subscribe "my.custom.event" do |name, started, finished, unique_id, data|
  puts data.inspect # {:this=>:data}
end
```

为什么使用？

Rails 内外，都有很多类似、可替换的技术。但这个方法，即能保证在同一项目内进行，又能使耦合度尽可能的降低。并且，它使用范围很广。

缺点：配置不方便，一启动就执行，并且更改要重启。默认对性能是有影响的，subscribe 并不是异步执行。

Rails 默认有很多 Instrumentation，你可以不写 instrument 直接 subscribe 它们。或者，你自己写 instrument，然后 subscribe。

控制台里运行 `ActiveSupport::Notifications.instrumenter` 可以查看有哪些 Instrumentation

Why would you do this yourself?

- Non-invasive and lightweight.
- No manipulations.
- Everything in one place.

实现：运用中间变量 notifier

> Note: 注意使用场景。这里并不是严格的发布者、订阅者模型，你从它们的方法名及默认参数就应该知道。

相关：

[Pssst... your Rails application has a secret to tell you](http://signalvnoise.com/posts/3091-pssst-your-rails-application-has-a-secret-to-tell-you)

[Digging Deep with ActiveSupport::Notifications](https://speakerdeck.com/nextmat/digging-deep-with-activesupportnotifications)

[#249 Notifications in Rails 3](http://railscasts.com/episodes/249-notifications-in-rails-3)

[ActiveSupport::Notifications, statistics and using facts to improve your site](http://www.reinteractive.net/posts/141-activesupport-notifications-statistics-and-using-facts-to-improve-your-site)

[Active Support Instrumentation](http://edgeguides.rubyonrails.org/active_support_instrumentation.html)
