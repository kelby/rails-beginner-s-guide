## Validations

在数据存入数据库存之前，有多个层面可以对数据做校验。包括客户端校验、Controller 级别的校验、Model 级别的校验和数据库本身做约束。它们各有利弊，在此不做讨论。

这里要讲的是 Model 级别的校验。

#### 类方法

常用：

```ruby
validate                # 调用方式一(不需要属性，也不需要校验器；直接做校验)
validates 和 validates! # 调用方式二(需要属性，也需要校验器；间接做校验)
validates_each          # 调用方式三(需要属性，不需要校验器；直接做校验)
validates_with          # 调用方式四(不需要属性，需要校验器；作用于所有对象，间接做校验)
```

除上述方法外，还有：

```ruby
validators # 查看所有可用的校验器
validators_on(*attributes) # 查看在某属性上使用了哪些校验器

clear_validators! # 清除校验器
```

以及：

```
attribute_method?(attribute)
```

`attribute_method?` 放在这里有点不太合适，它和校验没有多大关系。和 Attribute Methods 反而关系比较大。

#### Helper Methods

9 个校验方法，**语法糖**。 调用方式五(不需要属性，需要校验器；作用于某个具体对象，间接做校验)

```ruby
validates_format_of
validates_length_of & validates_size_of
validates_absence_of
validates_presence_of
validates_exclusion_of
validates_inclusion_of
validates_acceptance_of
validates_confirmation_of
validates_numericality_of
```

由于它们封装 `validates_with` 而来，既可当做类方法进行调用。
<br>
又由于它们具体实现时继承于 EachValidator，又可以当做 `validates` 的参数使用。

`validates_confirmation_of` 会为要校验的属性生成对应的读、写方法(x_confirmation、 x_confirmation=)

如：校验 password，会生成 password_confirmation 读方法和 password_confirmation= 写方法。

#### 实例方法

```ruby
errors

invalid?
valid? & validate

validates_with
```

`valid?` 校验是否通过，只有一个判断规则：当前 record 对象的 errors 值是否为空 errors.empty?  
所以自己写校验方法或者校验器的时候，请记得设置 errors 的值。

#### 调用方式比较

|           |    validate | validates 和 validates!  | validates_each | validates_with | validates_presence_of * |
| :-------- | :--------:| :--: | :--: | :--: | :--: |
| 属性       | 不需要 |  需要   | 需要 | 不需要 | 需要 |
| 校验器     |   不需要 |  需要  | 不需要 | 需要 | 不需要 |
| 直接、间接  |    直接 | 间接  | 直接 | 间接 | 直接 |

> Note: 
为了降低理解难度 validates_each 用到的 BlockValidator 不算为校验器；
<br>
validates_presence_of 代表着其它与之类似的方法。

上面说的都是类级别的校验，如果需要针对某个实例对象单独做额外的校验，可以使用实例方法 validates_with，参数和同名类方法一样。
