### Form Helper

Form Helper 是面向函数。

相对与 Form Builder 它更灵活，因为它没有限定对象。有时候我们'需要'这样做，而有时候我们没'必要'。

大部分封装 Tags::Xxx

名字 Form Helper 起得有一点不合适。

部分方法需要的参数 "object_name"，并不特指 record 对象，也可以是非 record 对象。

```
check_box
color_field

date_field
datetime_field
datetime_local_field

email_field

fields_for

form_for

label

file_field

hidden_field

month_field

number_field

password_field
phone_field

radio_button
range_field

search_field

telephone_field
text_area
text_field
time_field

url_field

week_field
```

form_for 和 fields_for 是另类，它们都可用于构建表单：

`form_for`
和 record 对象关联比较大。
<br>
`fields_for`
区别于 form_for，与 record 对象直接关联不大。
