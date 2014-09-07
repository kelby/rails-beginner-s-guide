# Model 之外

## AttributeMethods

AttributeMethods 给了我们大量的 accessor(访问器，读、写方法) 和 attribute 相关方法。用一种很方便的手法，为属性添加前缀、后缀。

使用步骤:

1. <tt>include ActiveModel::AttributeMethods</tt>
2. 调用 `attribute_method_suffix` 或 `attribute_method_prefix`，指明添加什么前缀、后缀
3. 在这之后，调用 `define_attribute_methods`，指明对哪些属性有效(默认是所有)
4. 定义一个方法，用 `attribute` 加上刚才的前缀、后缀做为方法名，实现其内容
5. 定义一个 `attributes` 方法。返回值是一个 hash，属性名做为 key，属性的值做为 value. 实际上可实现其读、写方法。

使用 Rails，有的步骤默认已经做了，所以使用上可以减少几个步骤，但其中的 2，4 是无论如何都不可省。

Rails 之外使用举例:

```ruby
class Person
  include ActiveModel::AttributeMethods

  # 前缀 + 后缀
  attribute_method_affix  prefix: 'reset_', suffix: '_to_default!'
  # 后缀
  attribute_method_suffix '_contrived?'
  # 前缀
  attribute_method_prefix 'clear_'
  # 要处理的属性
  define_attribute_methods :name

  attr_accessor :name

  # 供查询属性及值
  def attributes
    { 'name' => @name }
  end

  private
  # 根据上面的加的前缀 & 后缀定义并实现这里的方法。

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

使用 Rails 举例：

```ruby
class Person < ActiveRecord::Base
  attribute_method_affix :prefix => 'me_mateys_', :suffix => '_is_in_pirate?'

  private

  def me_mateys_attribute_is_in_pirate?(attr)
    send(attr).to_s =~ /\bYAR\b/i
  end
end

person = Person.find(1)
person.name                               #=> 'Paul Gillard'
person.profession                         #=> 'A Pirate, yar!'
person.me_mateys_name_is_in_pirate?       #=> false
person.me_mateys_profession_is_in_pirate? #=> true
```

主要用到的方法，都在示例里。ActiveRecord 基于它也实现了一个自己的 AttributeMethods.

[Reading Rails - Attribute Methods](http://monkeyandcrow.com/blog/reading_rails_attribute_methods/)<br>
[Building an Object Mapper: Override-able Accessors](http://www.railstips.org/blog/archives/2010/08/29/building-an-object-mapper-override-able-accessors/)

## Callbacks

`define_model_callbacks(*callbacks)` 默认情况下，我们在 model 类里定义的方法，是不能使用回调函数的(区别于过滤器)。使用这里提供的方法，可以定制。

## Dirty

跟踪对象值变化。其实，它们都属于脏数据，所以起这名字。但有时候很有用，例如某个字段一经生成不允许更改或者某字段每次更改要确保与上次不同。

```ruby
change(column_name, type, options = {})
changed?()
changed_attributes()
changes()
previous_changes()
```

Provides a way to track changes in your object in the same way as Active Record does.

The requirements for implementing ActiveModel::Dirty are:

* <tt>include ActiveModel::Dirty</tt> in your object.
* Call <tt>define_attribute_methods</tt> passing each method you want to
  track.
* Call <tt>attr_name_will_change!</tt> before each change to the tracked
  attribute.
* Call <tt>changes_applied</tt> after the changes are persisted.
* Call <tt>reset_changes</tt> when you want to reset the changes
  information.

A minimal implementation could be:

```ruby
class Person
  include ActiveModel::Dirty

  define_attribute_methods :name

  def name
    @name
  end

  def name=(val)
    name_will_change! unless val == @name
    @name = val
  end

  def save
    # do persistence work
    changes_applied
  end

  def reload!
    reset_changes
  end
end
```

当然，它本身也包含了 AttributeMethods 子模块，要不然不能精确跟踪到某属性。

## Serialization

`serializable_hash(options = nil)` 序列化操作，经测试和 `as_json` 结果一致。An `attributes` hash must be defined and should contain any attributes you need to be serialized.

此外，还有 module 及方法：

```ruby
# ActiveModel::Serializers::JSON
as_json
from_json

# ActiveModel::Serializers::Xml
to_xml
from_xml
```
