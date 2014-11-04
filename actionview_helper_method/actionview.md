# ActionView 辅助方法中与表单相关的部分

## Form Tag Helper

这里的方法与表单及 record 对象都没有直接相关，也就是说它们并不依赖于表单元素和 record 对象。把它们放到此章节，完全是因为它们所在的模块名字带 "Form".


**button 标签**

```
button_tag
```

**input 标签**

```
check_box_tag
color_field_tag
date_field_tag, datetime_field_tag, datetime_local_field_tag
email_field_tag
file_field_tag
hidden_field_tag
image_submit_tag
month_field_tag
number_field_tag
password_field_tag
phone_field_tag & telephone_field_tag
radio_button_tag
range_field_tag
search_field_tag
submit_tag
text_field_tag
time_field_tag
url_field_tag
utf8_enforcer_tag
week_field_tag
```

**fieldset 标签**

```
field_set_tag
```

**label 标签**

```
label_tag
```

**select 标签**

```
select_tag
```

**textarea 标签**

```
text_area_tag
```

**form 标签**

```
form_tag
```

上面并不是表单可以使用的所有方法，有一些是动态定义的。


```ruby
default_form_builder = ::ActionView::Helpers::FormBuilder 
builder = default_form_builder

object_name = 'product'
object = nil 
options = {} 

f = builder.new(object_name, object, self, options)
=> #<ActionView::Helpers::FormBuilder:0x007feaa896bc80
 @default_options={},
 @index=nil,
 @multipart=nil,
 @nested_child_index={},
 @object=nil,
 @object_name="product",
 @options={},
 @template=main>
```

## Form Options Helper

部分方法需要传递参数 "object"，并不是指 model 对象，可以是非 model 对象！

### 不可或缺

关键词：select

```
select(object, method, choices = nil, options = {}, html_options = {}, &block)

collection_select(object, method, collection, value_method, text_method, options = {}, html_options = {})

# 子关键词：optgroup
grouped_collection_select(object, method, collection, group_method, group_label_method, option_key_method, option_value_method, options = {}, html_options = {})

# 和 time_zone 有关
time_zone_select(object, method, priority_zones = nil, options = {}, html_options = {})
```

关键词：option

```
options_for_select(container, selected = nil)

options_from_collection_for_select(collection, value_method, text_method, selected = nil)
```

### 视情况而定

关键词：option

```
# 和 time_zone 有关
time_zone_options_for_select(selected = nil, priority_zones = nil, model = ::ActiveSupport::TimeZone)
```

关键词：optgroup

```
grouped_options_for_select(grouped_options, selected_key = nil, options = {})

option_groups_from_collection_for_select(collection, group_method, group_label_method, option_key_method, option_value_method, selected_key = nil)
```

关键词：checkbox

```
collection_check_boxes(object, method, collection, value_method, text_method, options = {}, html_options = {}, &block)
```

关键词：radio

```
collection_radio_buttons(object, method, collection, value_method, text_method, options = {}, html_options = {}, &block)
```

## Form Helper

部分方法需要的参数 "object_name"，并不是指 model 对象，可以是非 model 对象！

```
check_box
color_field
date_field
datetime_field
datetime_local_field
email_field

fields_for

file_field
form_for
hidden_field
label
month_field
number_field
password_field, phone_field
radio_button, range_field
search_field
telephone_field, text_area, text_field, time_field
url_field
week_field
```

## Form Builder

专用于 f.x 调用。

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
select, submit
time_select
time_zone_select

multipart=

emitted_hidden_id?

to_model
to_partial_path
```
