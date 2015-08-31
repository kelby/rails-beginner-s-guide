### 数据更新方法对比

| 方法 | 使用默认<br>Accessor? | 持久化对象? | 校验 | 回调 | 更新 <br>updated_at | Readonly <br>检查 | 单属性<br>多属性 |
| -- | -- | -- | -- | -- | :--: | :--: | -- |
| x= | 是 | 否 | - | - | - | - | 单 |
| write_attribute | 否 | 否 | - | - | - | - | 单 |
| update_attribute | 是 | 是 | 否 | 是 | 是 | 是 | 单 |
| assign_attributes & <br>attributes= | 是 | 否 | - | - | - | - | 多 |
| update & <br>update_attributes | 是 | 是 | 是 | 是 | 是 |是 | 多 |
| update_column | 否 | 是 | 否 | 否 | 否 | 是 | 单 |
| update_columns | 否 | 是 | 否 | 否 | 否 | 是 | 多 |
| User::update | 是 | 是 | 是 | 是 | 是 | 是 | 多 |
| User::update_all | 否 | 是 | 否 | 否 | 否 | 否 | 多 |

x= 表示直接赋值，其它几个是方法名

write_attribute(:name, ?) 等价于 user[:name]= ?

User::update 是类方法，直接封装了 User#update 实例方法，效果是一样的。

update & update_attributes 封装了 assign_attributes

update_column 直接封装了 update_columns

参考

[Different Ways to Set Attributes in ActiveRecord](http://www.davidverhasselt.com/set-attributes-in-activerecord/)
