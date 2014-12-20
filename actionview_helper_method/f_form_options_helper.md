## Form Options Helper

多用于处理集合。

常见于两个对象之间。

分为目的 和 手段。

比如：select_tag 是目的，options_for_select 是手段。

名字 Form Options Helper 起得有一点不合适。

---


部分方法需要传递参数 "object"，并不是指 model 对象，可以是非 model 对象！

### 不可或缺

关键词：select

```
select(object, method, choices = nil, options = {}, html_options = {}, &block)

collection_select(object, method, collection, value_method, text_method, options = {}, \n
                  html_options = {})

# 子关键词：optgroup
grouped_collection_select(object, method, collection, group_method, group_label_method, \n
                          option_key_method, option_value_method, options = {}, html_options = {})

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
time_zone_options_for_select(selected = nil, priority_zones = nil, \n
                             model = ::ActiveSupport::TimeZone)
```

关键词：optgroup

```
grouped_options_for_select(grouped_options, selected_key = nil, options = {})

option_groups_from_collection_for_select(collection, group_method, group_label_method, \n
                                         option_key_method, option_value_method, selected_key = nil)
```

关键词：checkbox

```
collection_check_boxes(object, method, collection, value_method, text_method, \n
                       options = {}, html_options = {}, &block)
```

关键词：radio

```
collection_radio_buttons(object, method, collection, value_method, text_method, \n
                         options = {}, html_options = {}, &block)
```
