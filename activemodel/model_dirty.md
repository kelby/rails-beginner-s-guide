## Dirty

跟踪对象的变化情况。其实，它们都属于脏数据，所以起这名字。但有时候很有用，例如某个字段一经生成不允许更改，或者某字段每次更改要确保与上次不同。

使用 ActiveModel::Dirty，需要:

* include ActiveModel::Dirty
* 调用 `define_attribute_methods` 参数是你想跟踪的属性
* 在改变属性的值之前，调用 `attr_name_will_change!` (把 attr_name 换成真正的属性名)
* 在改变属性的值之后，调用 `changes_applied`
* 如果你想重置上次改变的内容，调用 `reset_changes`

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

运用 **AttributeMethods** 生成的方法，能精确跟踪到某属性：

```
attribute_method_suffix '_changed?', '_change', '_will_change!', '_was'

# reset_attribute! 不再推荐使用，使用 restore_attribute! 代替
attribute_method_affix prefix: 'reset_', suffix: '!'

attribute_method_affix prefix: 'restore_', suffix: '!'
```

下面是对它们的解释

```
x_changed?     # 返回 true/false, x 属性有没有更改？
x_change       # 返回一个数组有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值
x_will_change! # 声明 x 元素已被更改，即使实际上它并没有更改。
x_was          # 根据 x_changed? 而来。如果有更改则返回 changed_attributes 里 x 属性的部分，没有更改则返回 x 属性的值

restore_x!     # 消除对 x 属性的更改
```

除上述方法外，还有

```
changed?       # 返回 true/false，整个对象有没有被更改？
changed        # 返回一个数组，所有被更改的属性
changes        # 返回一个 Hash. key 被更改的元素，value 是一个数组(有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值)
previous_changes   # 类似 changes, 区别是更新成功之后才使用。返回一个 Hash. key 被更改的元素，value 是一个数组(有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值)
changed_attributes # 返回一个 Hash. key 为被更改的元素，value 为其更改之前的值
restore_attributes # 清除更改数据

changes_applied
clear_changes_information
```

ActiveRecord 也有同名模块，它是对这里的 Dirty 的封装，并且它并没有对外提供 API.
