## Naming & Name

内省机制，主要负责**将对象转换成对应的字符串**。对于我们 Web 开发者来说不常用，但对于配合 Action Controller, Action View 工作很重要。

实例方法

```
model_name
```

类方法(调用方式奇怪，一般我们不会使用)

```
param_key
route_key
singular_route_key

singular     # 单数
plural       # 复数
uncountable? # 不可数？
```

方便我们把，字符串、符号、实例对象等转换成相关 model 进行处理。

`model_name` 返回的是 **Name 的实例对象**。Name 和普通字符串类似，但提供方法：

```
human
```

和

```
singular
plural
element
cache_key & collection

singular_route_key
route_key
param_key
i18n_key

name
```

可将字符串转换成对用户更友好的形式展现。

`model_name` 把"各种对象"转换成对应的"字符串"，而 View 要的正是"字符串"。比如以下几个方法：

ActionView

```
form_for

button
submit

fields_for

dom_id
dom_class
```

ActionController

```
wrap_parameters
```

ActionDispatch

```
polymorphic_url
polymorphic_path
```

相关 ActionController::ModelNaming 和 ActionView::ModelNaming.

> Note: 调用此模块的方式是 extend ActiveModel::Naming，不是 include；另外注意实例方法与类方法的调用方式是不一样的。
