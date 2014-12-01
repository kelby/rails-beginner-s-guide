## 数据更新方法对比

| 方法 | Uses Default Accessor | Saved to Database | Validations | Callbacks | Touches updated_at | Readonly check | 类、实例 | 单属性、多属性 |
| -- | -- | -- | -- | -- | -- | -- | -- | -- |
| attribute= | Yes | No | n/a | n/a | n/a | n/a | 7:2 | |
| write_attribute | No | No | n/a | n/a | n/a | n/a | 7:3 ||
| update_attribute | Yes | Yes | No | Yes | Yes | Yes | 7:4 ||
| attributes= | Yes | No | n/a | n/a | n/a | n/a | 7:5 ||
| update & update_attributes | Yes | Yes | Yes | Yes | Yes |Yes | 7:6 ||
| update_column | No | Yes | No | No | No | Yes | 7:7 ||
| update_columns | No | Yes | No | No | No | Yes | 7:8 ||
| User::update | Yes | Yes | Yes | Yes | Yes | Yes | 7:9 ||
| User::update_all | No | Yes | No | No | No | No | 7:10 ||

attribute= 表示直接赋值，其它几个是方法名

write_attribute(:name, x) 等价于 user[:name]= x

user.attributes = 等价于 user.assign_attributes

User::update 是类方法，直接封装了 User#update 实例方法，效果是一样的。

update & update_attributes 封装了 assign_attributes

update_column 直接封装了 update_columns

参考

[Different Ways to Set Attributes in ActiveRecord](http://www.davidverhasselt.com/set-attributes-in-activerecord/)
