### Form Builder

Form Builder 是面向对象。

你要修改的是 record 对象，途径是通过表单实现。
所以，record 对象和表单就必需有某种联系。
在这里，这种联系由 Form Builder 及其实例对象完成。

**源代码里，有 3 个来源：**

```
date_helper.rb
form_helper.rb
form_options_helper.rb
```

大部分是封装 @template

大部分以 f.xxx 的形式调用。

> Note: f 就是 FormBuilder 的实例对象，所以可以调用 FormBuilder 的实例方法。

**构建器怎么来？**

```ruby
# form_for 和 fields_for 调用 instantiate_builder

# 再调用
default_form_builder
  builder = ActionView::Base.default_form_builder
  ... ...
end

ActiveSupport.on_load(:action_view) do
  cattr_accessor(:default_form_builder) { ::ActionView::Helpers::FormBuilder }
end

# 简单理解 builder = FormBuilder

# 最后调用
builder.new(object_name, object, self, options)
```

**专用于 f.xxx 调用。**

```
button

check_box

collection_check_boxes
collection_radio_buttons
collection_select

date_select
datetime_select

fields_for

file_field

grouped_collection_select

hidden_field

label

radio_button

select

submit

time_select
time_zone_select
```
