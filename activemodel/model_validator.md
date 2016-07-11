

## Validator


校验可以简单分为 3 个层次：1）完全使用原生的；2）使用原生的 + 带条件判断；3）自定义校验器

自定义校验器，有两种方式：继承于 Validator 或 EachValidator.

#### 继承于 Validator

校验器在整个项目生命周期中只初始化一次。它针对的是整个对象，并且自动执行。这种方式，由 `validates_with` 加校验器名字的方式进行调用。

任何继承于 ActiveModel::Validator 的校验器都要实现 `validate` 方法，此方法接收要校验的 record 做为参数。然后，通过 `validates_with` 方法可以使用刚才定义的校验器。

```ruby
class MyValidator < ActiveModel::Validator
  # 继承于 Validator，需要实现 validate
  def validate(record)
    record  # => The person instance being validated
    options # => Any non-standard options passed to validates_with
  end
end

class Person
  include ActiveModel::Validations

  # 调用方式
  validates_with MyValidator
end
```

#### 继承于 EachValidator

实际上，推荐使用这种方式。这种方式，由 `validates` 以参数的方式进行调用。

任何继承于 ActiveModel::EachValidator 的校验器都要实现 `validate_each` 方法，此方法接收要校验的 record、attribute、value 做为参数。

```ruby
class TitleValidator < ActiveModel::EachValidator
  # 继承于 EachValidator，需要实现 validate_each
  def validate_each(record, attribute, value)
    record.errors.add attribute, 'must be Mr., or Mrs.' unless %w(Mr. Mrs.).include?(value)
  end
end
```

然后，通过 `validates` 方法(刚才定义的校验器做为参数之一)可以使用刚才定义的校验。

```ruby
class Person
  include ActiveModel::Validations
  attr_accessor :name

  # 调用方式
  validates :name, title: true
end
```

语法糖：

```ruby
ActiveRecord::Base.class_eval do
  def self.validates_date_of(*attr_names)
    validates_with TitleValidator, _merge_attributes(attr_names)
  end
end
```

```ruby
class Person < ActiveRecord::Base
  # ...

  # 调用方式
  validates_title_of :name
end
```

> Note: 
EachValidator 也继承于 Validator.
<br>
但 Rails 所有所有对外提供的校验器，都继承于 EachValidator.
