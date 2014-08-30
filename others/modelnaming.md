# ModelNaming

View 和 Controller 里有时候要处理 Model 里的对象，为了方便使用，做了一些约定。影响了 ActionView、ActionController 和 ActionDispatch.

ActionController::ModelNaming 和 ActionView::ModelNaming

提供了：

```ruby
convert_to_model
model_name_from_record_or_class
```

方便我们把，字符串、符号、实例对象等转换成相关 model 进行处理。

相关 ActiveModel::Name 和 ActiveModel::Naming

`model_name` 把"各种对象"转换成对应的"字符串"，而 View 要的正是"字符串"。

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
