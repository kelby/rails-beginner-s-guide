## Autosave Association

负责处理自动保存相关一切任务。

在 Builder::Association 里有构建：

```ruby
def self.define_callbacks(model, reflection)
  if dependent = reflection.options[:dependent]
    check_dependent_options(dependent)
    add_destroy_callbacks(model, reflection)
  end

  Association.extensions.each do |extension|
    # 这里构建
    extension.build model, reflection
  end
end
```

而 AssociationBuilderExtension 正是 Associations::Builder::Association.extensions 其中之一。

对应着的**私有类方法：**

```ruby
# 给指定的关联添加 autosave
add_autosave_association_callbacks
```

就是上面这方法添加的回调，非常重要。

常用实例方法：

```
mark_for_destruction

marked_for_destruction?
```

除此之外，还有实例方法：

```
changed_for_autosave?

destroyed_by_association
destroyed_by_association=

reload
```

私有类方法：

```ruby
define_non_cyclic_method
```
