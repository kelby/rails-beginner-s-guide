## Dirty

跟踪对象的变化情况。其实，它们都属于脏数据，所以起这名字。但有时候很有用，例如某个字段一经生成不允许更改，或者某字段每次更改要确保与上次不同。

```
# 都是实例方法
:x_changed? # 返回 true/false, x 属性有没有更改？
:x_change   # 返回一个数组有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值
:x_will_change!, # 声明 x 元素已被更改，即使实际上它并没有更改。

:changed_for_autosave?,
:_field_changed?,
:changed? # 返回 true/false，整个对象有没有被更改？
:changed  # 返回一个数组，所有被更改的属性
:changes  # 返回一个 Hash. key 被更改的元素，value 是一个数组(有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值)
:previous_changes   # 类似 changes, 区别是更新成功之后才使用。返回一个 Hash. key 被更改的元素，value 是一个数组(有两个元素，第 1 个为 x 属性更改前的值，第 2 个为 x 属性更改后的值)
:changed_attributes # 返回一个 Hash. key 为被更改的元素，value 为其更改之前的值
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
