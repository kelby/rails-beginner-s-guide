## Autosave Association

负责处理自动保存相关一切任务。

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

除上述方法外，还有私有类方法：

```
define_non_cyclic_method

# 给指定的关联添加 autosave
add_autosave_association_callbacks
```
