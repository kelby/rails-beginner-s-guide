## 数据更新方法对比

| 方法 | Uses Default Accessor | 是否会持久化对象 | Validations | Callbacks | Touches updated_at | Readonly check | 类、实例 | 单属性、多属性 |
| -- | -- | -- | :--: | :--: | -- | -- | -- | -- |
| x= | Yes | 否 | - | - | - | - | 实例 | 单 |
| write_attribute | No | 否 | - | - | - | - | 实例 | 单 |
| update_attribute | Yes | 是 | No | Yes | Yes | Yes | 实例 | 单 |
| assign_attributes & attributes= | Yes | 否 | - | - | - | - | 实例 | 多 |
| update & update_attributes | Yes | 是 | Yes | Yes | Yes |Yes | 实例 | 多 |
| update_column | No | 是 | No | No | No | Yes | 实例 | 单 |
| update_columns | No | 是 | No | No | No | Yes | 实例 | 多 |
| User::update | Yes | 是 | Yes | Yes | Yes | Yes | 类 | 多 |
| User::update_all | No | 是 | No | No | No | No | 类 | 多 |

x= 表示直接赋值，其它几个是方法名

write_attribute(:name, ?) 等价于 user[:name]= ?

User::update 是类方法，直接封装了 User#update 实例方法，效果是一样的。

update & update_attributes 封装了 assign_attributes

update_column 直接封装了 update_columns

参考

[Different Ways to Set Attributes in ActiveRecord](http://www.davidverhasselt.com/set-attributes-in-activerecord/)
