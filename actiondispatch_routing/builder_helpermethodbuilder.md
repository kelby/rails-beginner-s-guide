# builder HelperMethodBuilder

:action - Specifies the action prefix for the named route: :new or :edit. Default is no prefix.

:routing_type - Allowed values are :path or :url. Default is :url.

builder = ActionDispatch::Routing::PolymorphicRoutes::HelperMethodBuilder.plural '', 'url'

```
when Array
builder.handle_list record # 拆分处理，方式雷同
  when String, Symbol
builder.handle_string record # 直接使用此字符串
  when Class
builder.handle_class record # 小写，复数形式
  else
builder.handle_model record # 类型，小写，单数形式
```

以字符串拼接为手段，得到具体的路由 helper 方法。再求值，得到网址(本质也是字符串)。
