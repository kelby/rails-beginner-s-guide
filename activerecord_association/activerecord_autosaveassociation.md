## AutosaveAssociation

```
changed_for_autosave?

destroyed_by_association
destroyed_by_association=

mark_for_destruction
marked_for_destruction?

reload
```

除上述方法外，还有私有类方法：

```
define_non_cyclic_method

# 给指定的关联添加 autosave
add_autosave_association_callbacks
```

---

通过 `reflect_on_all_autosave_associations` 可以查看 autosave: true 的关联。
