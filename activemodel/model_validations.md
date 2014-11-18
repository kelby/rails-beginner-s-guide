## Validations

### 实例方法

在数据存入数据库存之前，有好几种方法可以做校验。包括客户端校验、Controller 级别的校验、Model 级别的校验和数据库本身做约束。

```ruby
errors

invalid?
valid? & validate
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
