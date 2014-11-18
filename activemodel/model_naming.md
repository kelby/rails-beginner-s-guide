## Naming

内省机制，主要负责**将对象转换成对应的字符串**。对于我们Web开发者来说不常用，但对于配合 ActionController, ActionView 工作很重要。

```ruby
model_name

param_key
route_key
singular_route_key

singular - 单数
plural - 复数
uncountable? - 不可数？
```

方便我们把，字符串、符号、实例对象等转换成相关 model 进行处理。

相关 ActionController::ModelNaming 和 ActionView::ModelNaming

`model_name` 把"各种对象"转换成对应的"字符串"，而 View 要的正是"字符串"。

```
form_for

button
submit

fields_for

dom_id

dom_class
```
