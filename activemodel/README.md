# ActiveModel

我们知道 MVC 结构里，Model(模型)层与数据库关系最紧密。现在我们是把 Model(模型层)再拆分一下，把对数据库真正有操作的这部分拆分成 ActiveRecord，把没有对数据库操作的这部分拆分成 ActiveModel。它们都属于 Model(模型层)。

做了这样的折分之后，如果我们觉得 ActiveRecord 不合味口，但又不想完全抛弃它。解决方案来了，那就是 ActionModel，ActiveModel 以接口的形式提供一系列方法给 model 类使用。

ActiveRecord 对于需要持久化的数据进行校验或处理，是很强大的。但如果我们不需要持久化数据，或者我们只需要很少的功能，如：提交表单数据在 Web 开发中，比较常见(例如填表单，给我们发送反馈意见) - 用户提交信息，无论校验是有效还是无效，都应该得到反馈。ActiveRecord 太重了，使用 ActiveModel 可以帮助我们减少复杂度。

> Note: 说明一下，ActiveModel 脱胎于 ActiveRecord，而不是依赖，它可以单独使用；反过来，ActiveRecord 依赖于 ActiveModel。

## Model 是重点

从普通Web开发者角度来看，ActiveModel 最重要的就是 Model。它又可分为: **校验** 和 **抽象ORM接口**，主要由 Naming，Conversion，Translation 和 Validations 等子模块组成。

> Note: Conversion & Validations 是 `include` 如果没有指明 ClassMethods，则引入的是实例方法；Naming & Translation 是 `extend` 默认引入的就已经是类方法。注意这点小差别，对于使用上会有帮助。

## Validations

## 实例方法

在数据存入数据库存之前，有好几种方法可以做校验。包括客户端校验、Controller 级别的校验、Model 级别的校验和数据库本身做约束。

```ruby
errors()
invalid?(context = nil)
valid?(context = nil)
validate(context = nil) - Alias for: valid?
validates_with(*args, &block) - 调用方式五(不需要属性，需要校验器；作用于某个具体对象，间接做校验)
```

校验是否通过 valid?，只有一个判断规则，当前 record 的 errors 是否为空 errors.empty? 所以自己写校验方法或者校验器的时候，请记得设置 errors 的值。

### Helper Methods

```ruby
validates_size_of
validates_format_of
validates_length_of
validates_absence_of
validates_presence_of
validates_exclusion_of
validates_inclusion_of
validates_acceptance_of
validates_confirmation_of
validates_numericality_of
```

封装 `validates_with` 而来，可当做类方法调用。由于具体实现时继承于 EachValidator，也可以当做 `validates` 的参数使用。

### Class Methods

```ruby
attribute_method?
clear_validators!
validate - 调用方式一(不需要属性，也不需要校验器；直接做校验)
validates - 调用方式二(需要属性，也需要校验器；间接做校验)
validates!
validates_each - 调用方式三(需要属性，不需要校验器；直接做校验)
validates_with - 调用方式四(不需要属性，需要校验器；作用于所有对象，间接做校验)
validators
validators_on
```

调用方式比较：

|           |    validate | validates  | validates_each | validates_with | validates_presence_of * |
| --------: | --------:| :--: | :--: | :--: | :--: |
| 属性       | 不需要 |  需要   | 需要 | 不需要 | 需要 |
| 校验器     |   不需要 |  需要  | 不需要 | 需要 | 不需要 |
| 直接、间接  |    直接 | 间接  | 间接 | 间接 | 直接 |

> *Note:* 为了降低理解难度 validates_each 用到的 BlockValidator 不算为校验器；validates_presence_of 是 validates_with 的语法糖，代表着其它类似方法；更多查看 [Why Use Validations?](http://edgeguides.rubyonrails.org/active_record_validations.html#why-use-validations-questionmark)

现在我们要说的是 Model 级别的校验。

## Validations Callbacks

在执行校验之前、之后做校验。

```
before_validation
after_validation
```

## Validator

自定义校验器，有两种方式，继承于 Validator 或 EachValidator.

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

直接继承于 Validator 的校验器在整个项目生命周期中只初始化一次。它针对的是整个对象，并且自动校验。实际上，推荐使用更更灵活的 EachValidator. 任何继承于 ActiveModel::EachValidator 的校验器都要实现 `validate_each` 方法，此方法接收要校验的 record、attribute、value 做为参数。

```ruby
class TitleValidator < ActiveModel::EachValidator
  # 继承于 EachValidator，需要实现 validate_each
  def validate_each(record, attribute, value)
    record.errors.add attribute, 'must be Mr., Mrs., or Dr.' unless %w(Mr. Mrs. Dr.).include?(value)
  end
end
```

然后，通过 `validates` 方法可以使用刚才定义的校验。

```ruby
class Person
  include ActiveModel::Validations
  attr_accessor :title

  validates :title, presence: true
end
```

方式主要有两种

一般自己写的校验方法比较少见，更别说自定义检验器了。出于习惯，我们往往会用 if...else 等语句来做判断；或者写成方法，以回调的形式调用。

- validate - 这种方式，由 `validate(*args, &block)` 后面加校验代码或校验方法。
- 自定义校验器 - 继承 ActiveModel::Validator 然后实现 `validate` 方法。这种方式，由 `validates_with` 加校验器名字的方式进行调用。
- 自定义校验 - 继承 ActiveModel::EachValidator 然后实现 `validate_each` 方法。这种方式，由 `validates` 里以参数的方式进行调用。

> Note: EachValidator 也继承于 Validator. Rails 内建的校验方法(或者说校验器)，都是继承于 EachValidator.

## Errors

对属性校验失败时的报错，是它的实例(不常用)。

```ruby
record.errors.class
=> ActiveModel::Errors
```

自己写校验方法或者校验器的时候，请务必设置 errors 的值。

## Naming

内省机制，主要负责**将对象转换成对应的字符串**。对于我们Web开发者来说不常用，但对于配合 ActionController, ActionView 工作很重要。

```ruby
model_name

param_key
route_key
singular_route_key

singular - 单数
plural - 复数
uncountable? - 不可数？
```

方便我们把，字符串、符号、实例对象等转换成相关 model 进行处理。

相关 ActionController::ModelNaming 和 ActionView::ModelNaming

`model_name` 把"各种对象"转换成对应的"字符串"，而 View 要的正是"字符串"。

```
form_for

button
submit

fields_for

dom_id

dom_class
```

## Conversion

```ruby
to_key
to_model
to_param
to_partial_path
```

`to_params` 默认用 id 做为 url 的一部分，这对用户体验和 SEO 都不友好，通常我们在 model 里覆盖此方法。


使用到它的一些方法：

```ruby
url_for

call

initialize

button_to
```

## Translation

`human_attribute_name(attribute, options = {})`

根据属性名，自动转换成对人类阅读更加友好的字符串。如 "first_name" 转换成 "First name".

使用场景非常非常有限。举例：View 根据字段，自动生成的内容；格式良好的 Errors 报错信息等。

## Lint::Tests

前面提到 ActiveModel 最重要的是 Model. 想要去掉 ActiveRecord，使用其它 ORM，基本的兼容性是要做的。如何检测基本的兼容性做到了？通过 Lint::Tests 可以检测即可。

它主要测试一些"必不可少"的基本方法：

```ruby
test_errors_aref
test_model_naming
test_persisted?
test_to_key
test_to_param
test_to_partial_path
```
