不要迷信这里提供的 helper，有的难读难懂，还不如自己写。
DHH 的這場 Talk 要傳達是「设计软件的重点：好读、易维护、以全局观思考」。

The Rails framework provides a large number of helpers for working with assets, dates, forms, numbers and model objects, to name a few. These helpers are available to all templates by default.

In addition to using the standard template helpers provided, creating custom helpers to extract complicated logic or reusable functionality is strongly encouraged. By default, each controller will include all helpers. These helpers are only accessible on the controller through `.helpers`

In previous versions of Rails the controller will include a helper whose name matches that of the controller, e.g., `MyController` will automatically include `MyHelper`. To return old behavior set `config.action_controller.include_all_helpers` to `false`.

## UrlHelper

可直接转化成 HTML：
button_to
link_to
mail_to

涉及逻辑：
link_to_if
link_to_unless
link_to_unless_current

current_page?(options)

## TranslationHelper

专有方法：
localize - Also aliased as: l
translate - Also aliased as: t

## TextHelper

concat 求值

current_cycle 循环内当前值
cycle 循环
reset_cycle 中断循环，从头开始

excerpt 引用、摘录(只显示某个词及其附近的语句，其余用 ... 代替)
highlight 高亮(默认是加 mark )
pluralize 复数形式

safe_concat 在 ActiveSupport 里也提供了 safe_concat 方法，如果可用则用；否则用 concat
simple_format 文本简单格式化
truncate 截取某个长度的文本
word_wrap 限制长度

## TagHelper
cdata_section
content_tag
escape_once
tag

## SanitizeHelper

sanitize
sanitize_css

strip_links
strip_tags

## RenderingHelper

render

## RecordTagHelper

content_tag_for

div_for

## OutputSafetyHelper

raw
safe_join

## NumberHelper

number_to_currency
number_to_human
number_to_human_size
number_to_percentage
number_to_phone
number_with_delimiter
number_with_precision

## JavaScriptHelper

escape_javascript - Also aliased as: j
javascript_tag

## FormTagHelper

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

## FormOptionsHelper

collection_check_boxes, collection_radio_buttons, collection_select
grouped_collection_select, grouped_options_for_select
option_groups_from_collection_for_select, options_for_select, options_from_collection_for_select
select
time_zone_options_for_select, time_zone_select

## FormHelper

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

## FormBuilder

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

## DebugHelper

debug

## DateHelper

date_select, datetime_select, distance_of_time_in_words, distance_of_time_in_words_to_now
select_date, select_datetime, select_day, select_hour, select_minute, select_month, select_second, select_time, select_year
time_ago_in_words, time_select, time_tag


## CsrfHelper

csrf_meta_tag, csrf_meta_tags

## CaptureHelper

capture, content_for, content_for?
provide

## CacheHelper

cache, cache_fragment_name, cache_if, cache_unless

## AtomFeedHelper

atom_feed

## AssetUrlHelper

直接转换

asset_path, asset_url, audio_path, audio_url
compute_asset_extname, compute_asset_host, compute_asset_path
font_path, font_url
image_path, image_url
javascript_path, javascript_url
path_to_asset, path_to_audio, path_to_font, path_to_image, path_to_javascript, path_to_stylesheet, path_to_video
stylesheet_path, stylesheet_url
url_to_asset, url_to_audio, url_to_font, url_to_image, url_to_javascript, url_to_stylesheet, url_to_video
video_path, video_url

## AssetTagHelper

audio_tag, auto_discovery_link_tag
favicon_link_tag
image_alt, image_tag
javascript_include_tag
stylesheet_link_tag
video_tag

## ActiveModelInstanceTag

content_tag - 多了error_wrapping (继承于 TagHelper)
error_message - 语法糖 error.messages
error_wrapping - 根据 errors 决定是如何封装
object
tag

## 其它

除了上述 helper 方法外，还有从 Controller delegate 过来的：

```ruby
delegate :request_forgery_protection_token, :params, :session, :cookies, :response, :headers,
         :flash, :action_name, :controller_name, :controller_path, :to => :controller
```

----------

此外，有一些 helper 很适用于 ActiveModel ...
