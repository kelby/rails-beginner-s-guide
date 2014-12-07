## Validator

自定义校验器，有两种方式，继承于 Validator 或 EachValidator.

- 继承 ActiveModel::Validator 然后实现 `validate` 方法。这种方式，由 `validates_with` 加校验器名字的方式进行调用。
- 继承 ActiveModel::EachValidator 然后实现 `validate_each` 方法。这种方式，由 `validates` 里以参数的方式进行调用。

任何继承于 ActiveModel::Validator 的校验器都要实现 `validate` 方法，此方法接收要校验的 record 做为参数。然后，通过 `validates_with` 方法可以使用刚才定义的校验器。

```ruby
class Person
  include ActiveModel::Validations
  validates_with MyValidator
end

class MyValidator < ActiveModel::Validator
  # 继承于 Validator，需要实现 validate
  def validate(record)
    record # => The person instance being validated
    options # => Any non-standard options passed to validates_with
  end
end
```

**直接继承于 Validator** 的校验器在整个项目生命周期中只初始化一次。它针对的是整个对象，并且自动校验。

**更灵活的 EachValidator** 实际上，推荐使用这种方式。任何继承于 ActiveModel::EachValidator 的校验器都要实现 `validate_each` 方法，此方法接收要校验的 record、attribute、value 做为参数。

```ruby
class TitleValidator < ActiveModel::EachValidator
  # 继承于 EachValidator，需要实现 validate_each
  def validate_each(record, attribute, value)
    record.errors.add attribute, 'must be Mr., Mrs., or Dr.' unless %w(Mr. Mrs. Dr.).include?(value)
  end
end
```

然后，通过 `validates` 方法(刚才定义的校验器做为参数之一)可以使用刚才定义的校验。

```ruby
class Person
  include ActiveModel::Validations
  attr_accessor :name

  validates :name, title: true
end
```

> Note: EachValidator 也继承于 Validator. Rails 内建的所有校验方法(或者说校验器)，都是继承于 EachValidator.
