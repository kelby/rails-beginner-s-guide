### Form Tag Helper

最接近 HTML 代码。

没有对象(不用对应 model)及类似概念。

封装 tag 或 content_tag 而来。

名字 Form Tag Helper 起得非常不合适。

这里的方法与表单及 record 对象都没有直接相关，也就是说它们并不依赖于表单元素和 record 对象。把它们放到此章节，完全是因为它们所在的模块名字带 "Form".

**button 标签**

```
button_tag
```

**input 标签**

```
check_box_tag

color_field_tag

date_field_tag
time_field_tag
datetime_field_tag
datetime_local_field_tag

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
