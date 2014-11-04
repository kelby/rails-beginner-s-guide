## Model 之外

## Attribute Methods

AttributeMethods 可以很方便给现有属性(可以是虚拟属性)添加前缀、后缀或前缀 + 后缀，然后得到新的方法。

使用步骤:

1. <tt>include ActiveModel::AttributeMethods</tt>
2. 调用 `attribute_method_prefix` 添加前缀，调用 `attribute_method_suffix` 添加后缀，调用 `attribute_method_affix` 添加前缀 + 后缀
3. 在这之后，调用 `define_attribute_methods`，指明对哪些属性有效(默认是所有)
4. 定义一个 `attributes` 方法。返回值是一个 hash，属性的名字做为 key，属性的值做为 value. 实际上可实现其读、写方法。
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

Rails 项目，有的步骤默认已经实现，所以使用上可以减少几个步骤，但其中的第 2，5 步是无论如何都不可省。

```ruby
class Person < ActiveRecord::Base
  attribute_method_affix :prefix => 'me_mateys_', :suffix => '_is_in_pirate?'

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

## Callbacks

`define_model_callbacks(*callbacks)` 
一般的，ORM 都会提供大量的回调函数，并且有些回调函数还存在共性。直接使用 ActiveSupport 那一套回调机制的话，略显麻烦，还会有重复代码。所以，ActiveModel 封装了 ActiveSupport，使得定义回调函数变得简单方便。

使用举例：

```ruby
class Person
  extend ActiveModel::Callbacks
 
  define_model_callbacks :update
 
  before_update :reset_me
 
  def update
    run_callbacks(:update) do
      # This method is called when update is called on an object.
    end
  end
 
  def reset_me
    # This method is called when update is called on an object as a before_update callback is defined.
  end
end
```

## Dirty

跟踪对象的变化情况。其实，它们都属于脏数据，所以起这名字。但有时候很有用，例如某个字段一经生成不允许更改，或者某字段每次更改要确保与上次不同。

```ruby
change(column_name, type, options = {})
changed?()
changed_attributes()
changes()
previous_changes()
```

使用 ActiveModel::Dirty，需要:

* <tt>include ActiveModel::Dirty</tt>
* 调用 <tt>define_attribute_methods</tt> 参数是你想跟踪的属性
* 在改变属性的值之前，调用 <tt>attr_name_will_change!</tt> (把 attr_name 换成真正的属性名)
* 在改变属性的值之后，调用 <tt>changes_applied</tt>
* 如果你想重置上次改变的内容，调用 <tt>reset_changes</tt>

举例:

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

来看看，都提供了什么方法：

```
# Instance Public methods 幂等方法
changed, changed?, changed_attributes, changes, previous_changes

# Instance Public methods 非幂等方法
restore_attributes

# Instance Private methods
changes_applied, clear_changes_information
```

当然，它本身也包含了 AttributeMethods 子模块，要不然不能精确跟踪到某属性。

ActiveRecord 也有同名模块，它是对这里的 Dirty 的封装，并且它并没有对外提供 API.

## Serialization

`serializable_hash(options = nil)` 序列化操作，经测试和 `as_json` 结果一致。使用 Serialization 需要 `record.respond_to? :attributes # => true` 因为转换的过程使用到相应 model 的 attributes 实例方法。

此外，还有 module 及方法：

```ruby
# ActiveModel::Serializers::JSON
as_json
from_json

# ActiveModel::Serializers::Xml
to_xml
from_xml
```
