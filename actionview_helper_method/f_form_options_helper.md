### Form Options Helper

多用于处理集合。

常见于两个对象之间。

分为目的 和 手段。

比如：select_tag 是目的，options_for_select 是手段。

名字 Form Options Helper 起得有一点不合适。

部分方法需要传递参数 "object"，并不特指 record 对象，可以是非 record 对象。

**最常用的几个方法**

关键词：select

```ruby
select

collection_select

# 子关键词：optgroup
grouped_collection_select

# 和 time_zone 有关
time_zone_select
```

关键词：option

```
options_for_select

options_from_collection_for_select
```

**相对而言，几个不太常用的方法**

关键词：option

```ruby
# 和 time_zone 有关
time_zone_options_for_select
```

关键词：optgroup

```
grouped_options_for_select

option_groups_from_collection_for_select
```

关键词：checkbox

```
collection_check_boxes
```

关键词：radio

```
collection_radio_buttons
```
