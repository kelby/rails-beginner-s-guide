## ~~Helper Method Builder~~

以字符串拼接为手段，得到具体的路由 helper 方法(不能直接得到网址！)。后续，可以再通过路由 helper 方法，得到网址。

:action - 其实是生成的方法所带的前缀，默认是没有的。Rails 使用了前缀 :new 和 :edit

:routing_type - 生成 :path 还是 :url, 默认是 :url.

```ruby
ActionDispatch::Routing::PolymorphicRoutes::HelperMethodBuilder.plural 'edit', 'url'

when Array
builder.handle_list record   # 拆分处理，方式雷同
  when String, Symbol
builder.handle_string record # 直接使用此字符串
  when Class
builder.handle_class record  # 小写，复数形式
  else
builder.handle_model record  # 类型，小写，单数形式
```

现在可以看到 Action View 和 Action Dispatch 里的 url_for 方法, 以及 Action Dispatch 里的 Polymorphic Routes 相关方法都封装了它，在某些参数情况下会调用到它。其它地方，没有使用。
