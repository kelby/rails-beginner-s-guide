# ActionView 辅助方法中与表单相关的部分

## Form Tag Helper

```
button_tag
check_box_tag, color_field_tag
date_field_tag, datetime_field_tag, datetime_local_field_tag
email_field_tag
field_set_tag, file_field_tag, form_tag
hidden_field_tag
image_submit_tag
label_tag
month_field_tag
number_field_tag
password_field_tag, phone_field_tag
radio_button_tag, range_field_tag
search_field_tag, select_tag, submit_tag
telephone_field_tag, text_area_tag, text_field_tag, time_field_tag
url_field_tag, utf8_enforcer_tag
week_field_tag
```

## Form Options Helper

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

collection_check_boxes 和 collection_radio_buttons 相比其它几个 helper, 是后来才提供的。

## Form Helper

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

```
button
check_box, collection_check_boxes, collection_radio_buttons, collection_select
date_select, datetime_select
emitted_hidden_id?
fields_for, file_field
grouped_collection_select
hidden_field
label
multipart=
new
radio_button
select, submit
time_select, time_zone_select, to_model, to_partial_path
```
