## Form Helper

Form Helper 是面向函数<br>
函数(f对象, 参数)。

Form Builder 是面向对象<br>
f对象.方法(参数)。

前者，更灵活。因为它没有限定对象，有时候我们'需要'这样做，而有时候我们没'必要'。

form_for 和 fields_for 是另类

大部分封装 Tags::Xxx

名字 Form Helper 起得有一点不合适。

---

部分方法需要的参数 "object_name"，并不是指 record 对象，可以是非 record 对象！

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
