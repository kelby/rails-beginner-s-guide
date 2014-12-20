## Primary Key

`primary_key` 主键(又称主关键字)。

是表中的一个或多个字段，它的值用于惟一地标识表中的某一条记录。默认是 'id' 属性，一般不会更改。

使用举例：

```ruby
class Project < ActiveRecord::Base
  self.primary_key = 'sysid'
end

# 或

class Project < ActiveRecord::Base
  def self.primary_key
    'foo_' + super
  end
end

Project.primary_key # => "foo_id"
```

除上述方法外，还有：

```
id
id=
id?
id_before_type_cast
id_was

to_key
```

和

```
dangerous_attribute_method?

define_method_attribute

primary_key
primary_key=

quoted_primary_key
```
