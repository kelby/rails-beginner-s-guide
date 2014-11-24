## Attribute Methods

AttributeMethods 可以很方便给现有属性(可以是虚拟属性)添加前缀、后缀或前缀 + 后缀，然后得到新的方法。

使用步骤:

1. include ActiveModel::AttributeMethods
2. 调用 `attribute_method_prefix` 添加前缀，调用 `attribute_method_suffix` 添加后缀，调用 `attribute_method_affix` 添加前缀 + 后缀
3. 在这之后，调用 `define_attribute_methods`，指明对哪些属性有效(默认是所有)
4. 定义一个 `attributes` 方法。返回值是一个 hash，属性的名字做为 key，属性的值做为 value. 实际上可实现其读、写方法，相当于 attr_accessor
5. 用 attribute 加上刚才的前缀、后缀做为方法名，定义一个新的方法

单独使用 AttributeMethods 举例:

```ruby
class Person
  include ActiveModel::AttributeMethods

  # 前缀
  attribute_method_prefix 'clear_'
  # 后缀
  attribute_method_suffix '_contrived?'
  # 前缀 + 后缀
  attribute_method_affix  prefix: 'reset_', suffix: '_to_default!'

  # 要处理的属性
  define_attribute_methods :name

  attr_accessor :name

  # 供查询属性及值
  def attributes
    { 'name' => @name }
  end

  private
  # 用 attribute 代替上面要处理的属性，加上前缀、后缀得到新的方法名，然后实现它们

  def attribute_contrived?(attr)
    true
  end

  def clear_attribute(attr)
    send("#{attr}=", nil)
  end

  def reset_attribute_to_default!(attr)
    send("#{attr}=", 'Default Name')
  end
end
```

Rails 项目，有的步骤默认已经实现，所以使用上可以减少几个步骤，但其中的第 2，5 步是无论如何都不可省。

```ruby
class Person < ActiveRecord::Base
  attribute_method_affix prefix: 'me_mateys_', suffix: '_is_in_pirate?'

  private

  def me_mateys_attribute_is_in_pirate?(attr)
    send(attr).to_s =~ /\bYAR\b/i
  end
end

person = Person.find(1)
person.name                               # => 'Paul Gillard'
person.profession                         # => 'A Pirate, yar!'
person.me_mateys_name_is_in_pirate?       # => false
person.me_mateys_profession_is_in_pirate? # => true
```

主要用到的方法，都在示例里。ActiveRecord 基于它也实现了一个自己的 AttributeMethods.
